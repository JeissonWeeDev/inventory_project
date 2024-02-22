<script>
  // Recursos
  import tagStep1 from "$lib/assets/tagUpStep1.svg";
  import iconCookies from "$lib/assets/icon_cookies.svg";

  let currentStep = 3;
  const totalSteps = 3;

  /* Variables para la referencia de altura, esto es para que cada paso tenga la altura del contendio automaticamente (Es un problema de diseño) */
  let stepRef1, stepRef2, stepRef3;

  let acceptButtonEnabled;

  let formData = {
    username: "",
    email: "",
    acceptTerms: false,
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
</script>

<div class="container">
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
        tenerte a bordo. Este proceso te guiará paso a paso para personalizar tu
        experiencia.
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
        y <a href="/privacidad">Política de Privacidad</a>, las cuales explican
        cómo recopilamos, utilizamos y salvaguardamos tus datos.
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
      class={`step-content ${currentStep === 3 ? "step3" : ""}`}
      bind:this={stepRef3}
      style={`opacity: ${currentStep === 3 ? 1 : 0}`}
    >
      <form class="form-main" action="" method="post">
        <h2>¡Registrate para comenzar!</h2>

        <label class="form-label-inp" for="username">Nombre de Usuario: <span>*</span></label>
        <input
          class="form-inp"
          bind:value={formData.username}
          type="text"
          id="username"
          required
          on:input={handleInputChange}
          placeholder="Escribe tu nombre"
        />

        <label class="form-label-inp" for="email">Correo Electrónico: <span>*</span></label>
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
        <label class="check-cont">
          <input
            bind:checked={formData.acceptTerms}
            type="checkbox"
            on:change={handleInputChange}
          />
          <span>Acepto términos de uso</span>
        </label>
      </form>
    </div>
  </div>

  <div class="circle-group">
    {#each Array(totalSteps) as _, index}
      <div class={`circle ${currentStep === index + 1 ? "active" : ""}`}></div>
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
        on:click={nextStep}
        disabled={!acceptButtonEnabled}
        class:btn-accept-des={!acceptButtonEnabled}
        class:btn-accept={acceptButtonEnabled}
      >
        Aceptar
      </button>
    {/if}
  </div>
</div>

<style>
  .container {
    overflow: hidden;
  }

  .steps-container {
    display: flex;
    width: auto;
    margin-bottom: 1.5rem;
    transition: all 0.25s ease;
  }

  .step-content {
    flex: 0 0 100%;
    width: 100%;
    height: max-content;
    padding: 0 0.5rem;
    transition: opacity 0.5s ease-in-out;
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
    max-width: 243px;
    font-size: 2rem;
    font-weight: 600;
    margin-bottom: 1rem;
  }
  .form-label-inp {
    font-size: 14px;
    font-weight: 500;
    color: rgba(245, 245, 245, 0.8);
    margin-bottom: .5rem;
  }
  .form-label-inp span {
    color: #FE0000;
  }

  .form-inp {
    height: 2.5rem;
    border-radius: 1.5rem;
    border: 1px solid rgba(255, 255, 255, 0.16);
    font-size: 16px;
    font-weight: 400;
    color: rgba(255, 255, 255, 0.92);
    padding: 0 1rem;
    margin-bottom: 1rem;
  }
  .form-inp::placeholder {
    color: rgba(255, 255, 255, 0.24);
  }

  .check-cont {
    border: 1px solid red;
    margin-top: 1rem;
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
    color: black;
    background-color: white; /* Cambia el color según tu preferencia */
    padding: 8px 16px;
    border: none;
    border-radius: 4px;
    margin: 0 5px;
    cursor: pointer;
  }

  .btn-accept-des {
    opacity: 0.5;
    cursor: not-allowed;
  }
</style>
