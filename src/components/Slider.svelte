<script>
  import CryptoJS from "crypto-js";
  import { openDB } from "idb";
  import LoadingIcon from "./icons/LoadingIcon.svelte";
  import ReadyIson from "./icons/ReadyIson.svelte";

  // Recursos ---------
  import tagStep1 from "$lib/assets/tagUpStep1.svg";
  import iconCookies from "$lib/assets/icon_cookies.svg";

  // Valores de entrada ---------
  export let closeModal;

  // Informacion de los slider y sus pasos ---------
  let currentStep = 1;
  const totalSteps = 3;

  /* Variables para la referencia de altura, esto es para que cada paso tenga la altura del contendio automaticamente (Es un problema de diseño) */
  let stepRef1, stepRef2, stepRef3;
  let acceptButtonEnabled; // Estado del boton de guardado en registro

  /* Estructura para el registro de datos de usuario */
  let formData = {
    username: "",
    email: "",
    acceptContri: false,
  };

  /* Estados de guardado de datos */
  let savingDataState = {
    savingProcess: false,
    loadingState: false,
    readyProcess: false,
  };

  const nextStep = () => {
    if (currentStep < totalSteps) {
      if (currentStep === 3 && !acceptButtonEnabled) {
        return;
      }
      currentStep += 1;
    }
  };
  const prevStep = () => {
    if (currentStep > 1) {
      currentStep -= 1;
    }
  };

  const handleInputChange = (event) => {
    formData = {
      ...formData,
      [event.target.id]:
        event.target.type === "checkbox"
          ? event.target.checked
          : event.target.value,
    };
    acceptButtonEnabled =
      formData.username.trim() !== "" && formData.email.trim() !== "";
  };

  const secretKey = "clave-secreta"; // Puedes generar una clave única para cada usuario

  const encryptField = (field) => {
    const fieldString = JSON.stringify(field);
    const encryptedField = CryptoJS.AES.encrypt(
      fieldString,
      secretKey
    ).toString();
    return encryptedField;
  };

  const saveToIndexedDB = async () => {
    const encryptedUsername = encryptField(formData.username);
    const encryptedEmail = encryptField(formData.email);
    const encryptedAcceptContri = encryptField(formData.acceptContri);

    const db = await openDB("userDataDB", 1, {
      upgrade(db) {
        db.createObjectStore("userDataStore");
      },
    });

    const tx = db.transaction("userDataStore", "readwrite");
    const userDataStore = tx.objectStore("userDataStore");
    userDataStore.put(encryptedUsername, "username");
    userDataStore.put(encryptedEmail, "email");
    userDataStore.put(encryptedAcceptContri, "acceptContri");

    await tx.done;

    // Llamada a la función de consulta después del guardado
    retrieveFromIndexedDB();
  };

  /* Esta funcion es de prueba para probar el sistema de encriptado de las datos y su guardado */
  const retrieveFromIndexedDB = async () => {
    const db = await openDB("userDataDB", 1);
    const tx = db.transaction("userDataStore", "readonly");
    const userDataStore = tx.objectStore("userDataStore");

    const encryptedUsername = await userDataStore.get("username");
    const encryptedEmail = await userDataStore.get("email");
    const encryptedAcceptContri = await userDataStore.get("acceptContri");

    if (encryptedUsername) {
      const decryptedUsernameBytes = CryptoJS.AES.decrypt(
        encryptedUsername,
        secretKey
      );
      const decryptedUsername = JSON.parse(
        decryptedUsernameBytes.toString(CryptoJS.enc.Utf8)
      );

      console.log("Nombre de Usuario desencriptado:", decryptedUsername);
    } else {
      console.error(
        "No se encontró el nombre de usuario cifrado en IndexedDB."
      );
    }

    if (encryptedEmail) {
      const decryptedEmailBytes = CryptoJS.AES.decrypt(
        encryptedEmail,
        secretKey
      );
      const decryptedEmail = JSON.parse(
        decryptedEmailBytes.toString(CryptoJS.enc.Utf8)
      );

      console.log("Correo Electrónico desencriptado:", decryptedEmail);
    } else {
      console.error(
        "No se encontró el correo electrónico cifrado en IndexedDB."
      );
    }

    if (encryptedAcceptContri) {
      const decryptedAcceptTermsBytes = CryptoJS.AES.decrypt(
        encryptedAcceptContri,
        secretKey
      );
      const decryptedAcceptTerms = JSON.parse(
        decryptedAcceptTermsBytes.toString(CryptoJS.enc.Utf8)
      );

      console.log("Accept Terms desencriptado:", decryptedAcceptTerms);
    } else {
      console.error(
        "No se encontró el valor Accept Terms cifrado en IndexedDB."
      );
    }
  };

  const handleClickForm = async () => {
    const isUsernameValid = isNonEmptyString(formData.username);
    const isEmailValid =
      isNonEmptyString(formData.email) && isValidEmail(formData.email);

    if (!isUsernameValid && !isEmailValid) {
      console.error(
        "Por favor, ingrese un nombre de usuario y un correo electrónico válidos."
      );
      return;
    }
    if (!isUsernameValid) {
      console.error("Por favor, ingrese un nombre de usuario válido.");
      return;
    }
    if (!isEmailValid) {
      console.error("El formato del correo electrónico no es válido.");
      return;
    }

    try {
      savingDataState.savingProcess = true;
      savingDataState.loadingState = true;
      
      await saveToIndexedDB();
      await new Promise(resolve => setTimeout(resolve, 1000));

      savingDataState.loadingState = false
      savingDataState.readyProcess = true;

      await new Promise(resolve => setTimeout(resolve, 600));
      closeModal();
    } catch {
      savingDataState.savingProcess = false;
      console.error("Error al guardar datos");
    }
  };

  const isNonEmptyString = (value) => typeof value === "string" && value.trim() !== "";

  const isValidEmail = (email) => {
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return emailRegex.test(email.trim());
  };
</script>

<div class="container">
  {#if !savingDataState.savingProcess}
    <div
      class="steps-container"
      style={`
      transform: translateX(-${(currentStep - 1) * 100}%);
      height: ${
        currentStep === 1 && stepRef1
          ? stepRef1.clientHeight + "px"
          : currentStep === 2 && stepRef2
            ? stepRef2.clientHeight + "px"
            : currentStep === 3 && stepRef3
              ? stepRef3.clientHeight + "px"
              : "auto"
      };`}
    >
      <div
        class={`step-content step1`}
        bind:this={stepRef1}
        style={`opacity: ${currentStep === 1 ? 1 : 0}`}
      >
        <img src={tagStep1} alt="Simplifica tu flujo de inventario" />
        <h2>Bienvenido a FlowStock: Simplificando la Gestión de Inventarios</h2>
        <p class="tx_normal">
          ¡Bienvenido a FlowStock! Tu solución intuitiva para simplificar y
          optimizar tu flujo de trabajo de inventario. Estamos emocionados de
          tenerte a bordo. Este proceso te guiará paso a paso para personalizar
          tu experiencia.
        </p>
      </div>
      <div
        class={`step-content step2`}
        bind:this={stepRef2}
        style={`opacity: ${currentStep === 2 ? 1 : 0}`}
      >
        <img src={iconCookies} alt="Icon Cookies" />
        <h2>Aviso de Cookies y Datos Locales</h2>
        <p class="tx_normal">
          Al continuar navegando, aceptas nuestra <a href="/politica"
            >Política de Uso</a
          >
          y <a href="/privacidad">Política de Privacidad</a>, las cuales
          explican cómo recopilamos, utilizamos y salvaguardamos tus datos.
        </p>
        <svg
          width="32"
          height="32"
          viewBox="0 0 40 40"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
          ><rect
            x=".5"
            y=".5"
            width="39"
            height="39"
            rx="19.5"
            stroke="#999"
          /><mask
            id="a"
            style="mask-type:luminance"
            maskUnits="userSpaceOnUse"
            x="10"
            y="10"
            width="20"
            height="20"><path d="M30 10H10v20h20V10Z" fill="#fff" /></mask
          ><g mask="url(#a)"
            ><mask
              id="b"
              style="mask-type:alpha"
              maskUnits="userSpaceOnUse"
              x="4"
              y="14"
              width="19"
              height="18"
              ><path
                d="M16.653 31.015 4.926 22.892l6.13-8.85 11.727 8.123-6.13 8.85Z"
                fill="#fff"
              /></mask
            ><g mask="url(#b)"
              ><mask
                id="c"
                style="mask-type:alpha"
                maskUnits="userSpaceOnUse"
                x="4"
                y="14"
                width="19"
                height="18"
                ><path
                  d="M16.653 31.015 4.926 22.892l6.13-8.85 11.727 8.123-6.13 8.85Z"
                  fill="#fff"
                /></mask
              ><g mask="url(#c)"
                ><mask
                  id="d"
                  style="mask-type:alpha"
                  maskUnits="userSpaceOnUse"
                  x="10"
                  y="14"
                  width="13"
                  height="14"
                  ><path
                    d="m12.081 14.752 9.677 6.703a1.248 1.248 0 0 1-.278 2.196l-9.442 3.453c-.431.151-.9.09-1.27-.165a1.381 1.381 0 0 1-.594-1.126l-.05-10.029c-.002-.468.254-.89.662-1.11a1.256 1.256 0 0 1 1.295.078Z"
                    fill="#fff"
                  /></mask
                ><g mask="url(#d)"
                  ><path
                    d="m18.38 32.975 9.952-14.367-14.248-9.869-9.952 14.367 14.248 9.87Z"
                    fill="#999"
                  /></g
                ></g
              ></g
            ><mask
              id="e"
              style="mask-type:alpha"
              maskUnits="userSpaceOnUse"
              x="17"
              y="8"
              width="19"
              height="18"
              ><path
                d="m23.346 8.985 11.728 8.123-6.13 8.85-11.728-8.124 6.13-8.85Z"
                fill="#fff"
              /></mask
            ><g mask="url(#e)"
              ><mask
                id="f"
                style="mask-type:alpha"
                maskUnits="userSpaceOnUse"
                x="17"
                y="8"
                width="19"
                height="18"
                ><path
                  d="m23.346 8.985 11.728 8.123-6.13 8.85-11.728-8.124 6.13-8.85Z"
                  fill="#fff"
                /></mask
              ><g mask="url(#f)"
                ><mask
                  id="g"
                  style="mask-type:alpha"
                  maskUnits="userSpaceOnUse"
                  x="17"
                  y="12"
                  width="13"
                  height="14"
                  ><path
                    d="m27.919 25.248-9.677-6.703a1.248 1.248 0 0 1 .278-2.196l9.442-3.453c.43-.151.9-.09 1.27.165.368.256.59.674.594 1.126l.05 10.028c.002.469-.255.89-.662 1.11a1.256 1.256 0 0 1-1.295-.077Zm.652-11.234a.222.222 0 0 0-.202-.026l-9.442 3.454c-.01.001-.049.019-.06.073a.08.08 0 0 0 .037.087l9.677 6.703c.012.008.041.029.094.004.053-.026.045-.066.048-.082L28.66 14.19a.211.211 0 0 0-.089-.176Z"
                    fill="#fff"
                  /></mask
                ><g mask="url(#g)"
                  ><path
                    d="m21.62 7.024-9.952 14.368 14.248 9.869 9.952-14.367-14.248-9.87Z"
                    fill="#999"
                  /></g
                ></g
              ></g
            ></g
          ></svg
        >
      </div>
      <div
        class={`step-content step3`}
        bind:this={stepRef3}
        style={`opacity: ${currentStep === 3 ? 1 : 0}`}
      >
        <form class="form-main" action="" method="post">
          <h2>¡Registrate para comenzar!</h2>

          <label class="form-label-inp" for="username"
            >Nombre de Usuario: <span>*</span></label
          >
          <input
            class="form-inp"
            bind:value={formData.username}
            type="text"
            id="username"
            required
            on:input={handleInputChange}
            placeholder="Escribe tu nombre"
          />

          <label class="form-label-inp" for="email"
            >Correo Electrónico: <span>*</span></label
          >
          <input
            class="form-inp"
            bind:value={formData.email}
            type="email"
            id="email"
            required
            on:input={handleInputChange}
            placeholder="Escribe tu correo electronico"
          />
          <div class="ry-div" />

          <div class="check-cont">
            <div>
              <input
                class="check-input"
                type="checkbox"
                id="switch"
                bind:checked={formData.acceptTerms}
                on:change={handleInputChange}
              />
              <label class="check-label" for="switch">T</label>
            </div>
            <p class="check-txt">
              <span>Activar contribución automatica</span> (Para la recoleccion de
              errores)
            </p>
          </div>

          <div class="cont-help">
            <p>¿Necesitas ayuda? <a href="/ayuda">Ayuda y soporte</a></p>
          </div>
        </form>
      </div>
    </div>

    <div class="circle-group">
      {#each Array(totalSteps) as _, index}
        <div
          class={`circle ${currentStep === index + 1 ? "active" : ""}`}
        ></div>
      {/each}
    </div>

    <div class="navigation-buttons">
      {#if currentStep > 1}
        <button on:click={prevStep} class="btn-normal-ant">Anterior</button>
      {/if}
      {#if currentStep < 3}
        <button
          on:click={nextStep}
          disabled={currentStep === totalSteps}
          class="btn-normal">Siguiente</button
        >
      {:else}
        <button
          on:click={handleClickForm}
          disabled={!acceptButtonEnabled}
          class:btn-accept-des={!acceptButtonEnabled}
          class:btn-accept={acceptButtonEnabled}
        >
          Guardar
        </button>
      {/if}
    </div>
  {:else}
    <div class="cont-process">
      {#if savingDataState.loadingState}
        <LoadingIcon width={40} height={40} timeAnimation={"0.3s"} />
      {/if}
      {#if savingDataState.readyProcess}
        <ReadyIson width={40} height={40}/>
      {/if}
    </div>
  {/if}
</div>

<style>
  .container {
    height: max-content;
    overflow: hidden;
  }

  .steps-container {
    display: flex;
    width: auto;
    margin-bottom: 1.5rem;
    transition: height 0.2s ease;
  }

  .step-content {
    flex: 0 0 100%;
    width: 100%;
    height: max-content;
    padding: 0 0.5rem;
    transition: all 0.2s ease;
    overflow-y: hidden;
    z-index: 11;
  }

  /* Estilos general de texto en slide */
  .tx_normal {
    font-size: 15px;
    font-weight: 400;
    color: rgba(255, 255, 255, 0.6);
    text-align: center;
    margin: 0 0.5rem;
  }
  .tx_normal a {
    color: var(--color-main);
  }
  /* Estilos de raya divisora */
  .ry-div {
    border-top: 1px solid rgba(245, 245, 245, 0.08);
    width: auto;
  }

  /* ----- Estilos de paso 1 ----- */
  .step1 {
    height: max-content;
  }
  .step1 img:nth-child(1) {
    width: 100%;
    height: 1.8rem;
    margin-bottom: 0.5rem;
  }
  .step1 h2:nth-child(2) {
    font-size: 2rem;
    font-weight: 700;
    text-align: center;
    background: radial-gradient(
      50.13% 100% at 50.13% 0%,
      #f5f5f5 0%,
      rgba(245, 245, 245, 0.6) 100%
    );
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 1rem;
  }

  .step2 {
    width: auto;
    height: max-content;
  }
  .step2 img:nth-child(1) {
    width: 100%;
    height: 5rem;
    margin-bottom: 0.5rem;
  }
  .step2 h2:nth-child(2) {
    font-size: 2rem;
    font-weight: 700;
    text-align: center;
    background: radial-gradient(
      50.13% 100% at 50.13% 0%,
      #f5f5f5 0%,
      rgba(245, 245, 245, 0.6) 100%
    );
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 1rem;
  }
  .step2 svg {
    width: 100%;
    height: 2rem;
    margin-top: 1rem;
  }

  .step3 {
    height: max-content;
    padding: 2rem 1rem 0;
  }
  .form-main {
    display: flex;
    flex-direction: column;
  }
  .step3 h2 {
    background: radial-gradient(
      49.87% 100% at 0% 0%,
      #fff 0%,
      rgba(255, 255, 255, 0.5) 100%
    );
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    font-size: 2rem;
    font-weight: 600;
    margin-bottom: 1rem;
  }
  .form-label-inp {
    font-size: 14px;
    font-weight: 500;
    color: rgba(245, 245, 245, 0.8);
    margin-bottom: 0.5rem;
  }
  .form-label-inp span {
    color: #fe0000;
  }

  .form-inp {
    height: 2.5rem;
    border-radius: 1rem;
    --inp-cl: rgba(255, 255, 255, 0.16);
    border: 1px solid var(--inp-cl);
    font-size: 16px;
    font-weight: 400;
    color: rgba(255, 255, 255, 0.92);
    padding: 0 1rem;
    margin-bottom: 1rem;
  }
  .form-inp::placeholder {
    color: rgba(255, 255, 255, 0.24);
  }
  .form-inp:hover {
    --inp-cl: rgba(255, 255, 255, 0.32);
  }
  .form-inp:focus {
    --inp-cl: rgba(255, 255, 255, 0.32);
  }
  .form-inp:valid {
    --inp-cl: var(--ready-color);
  }

  /* Estilos del checkbox */
  .check-cont {
    display: flex;
    margin-top: 1rem;
  }
  .check-cont div:nth-child(1) {
    display: flex;
    margin-right: 0.5rem;
  }
  .check-input {
    height: 0;
    width: 0;
    visibility: hidden;
  }

  .check-label {
    cursor: pointer;
    text-indent: -9999px;
    width: 42px;
    height: 24px;
    display: block;
    border-radius: 1rem;
    position: relative;
    --act-color: rgba(255, 255, 255, 0.24);
    border: 1px solid var(--act-color);
    overflow: hidden;
  }
  .check-label:hover {
    --act-color: rgba(255, 255, 255, 0.32);
  }
  .check-label:after {
    content: "";
    position: absolute;
    top: 2px;
    left: 2px;
    width: 18px;
    height: 18px;
    background: var(--act-color);
    border-radius: 50%;
    transition: 0.2s ease;
  }

  .check-input:checked + .check-label {
    --act-color: var(--ready-color);
  }
  .check-input:checked + .check-label:after {
    left: calc(100% - 2px);
    transform: translateX(-100%);
  }

  .check-txt {
    font-weight: 400;
    font-size: 15px;
    color: rgba(245, 245, 245, 0.6);
  }
  .check-txt span {
    font-weight: 500;
    color: rgba(245, 245, 245, 0.8);
  }

  .cont-help {
    display: flex;
    justify-content: center;
    margin-top: 2rem;
    width: auto;
  }
  .cont-help p {
    font-size: 13px;
    font-weight: 400;
    color: rgba(255, 255, 255, 0.5);
  }
  .cont-help p a {
    color: var(--color-main);
  }

  /* Estilos de circulos guia */
  .circle-group {
    border: 1px solid rgba(245, 245, 245, 0.16);
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 0.5rem;
    width: max-content;
    height: 20px;
    margin: auto;
    padding: 6px;
    border-radius: 1rem;
  }
  .circle {
    width: 0.5rem;
    height: 0.5rem;
    border-radius: 50%;
    background-color: rgba(245, 245, 245, 0.5);
    display: inline-block;
  }
  .active {
    background-color: var(--color-main);
  }

  .navigation-buttons {
    display: flex;
    gap: 1rem;
    width: auto;
    height: 2.5rem;
    margin: 1rem;
  }

  .btn-normal {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 2.5rem;
    background-color: var(--color-main);
    border-radius: 1rem;
    cursor: pointer;
    font-size: 1rem;
    font-weight: 500;
    color: #000;
  }
  .btn-normal-ant {
    cursor: pointer;
    padding: 0 2rem;
    background-color: rgba(245, 245, 245, 0.08);
    border-radius: 1rem;
    font-size: 1rem;
    font-weight: 400;
    color: rgba(245, 245, 245, 0.8);
  }

  .btn-accept {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    font-size: 16px;
    font-weight: 500;
    color: black;
    background-color: var(--color-main);
    border: 1rem;
    border-radius: 1rem;
    cursor: pointer;
  }
  .btn-accept-des {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    font-size: 16px;
    font-weight: 500;
    color: rgba(255, 255, 255, 0.24);
    border: 1px solid rgba(255, 255, 255, 0.24);
    border-radius: 1rem;
    cursor: not-allowed;
  }

  /* Contenedor de proceso de guardado */
  .cont-process {
    display: flex;
    justify-content: center;
    height: max-content;
    padding: 2rem 0;
    margin: 0 0 2rem 0;
  }
</style>
