<script>
  let currentStep = 1;
  const totalSteps = 3; // Puedes ajustar según la cantidad de pasos que necesites

  const nextStep = () => {
    if (currentStep < totalSteps) {
      currentStep += 1;
    }
  };

  const prevStep = () => {
    if (currentStep > 1) {
      currentStep -= 1;
    }
  };
</script>

<style>
  .container {
    border: 1px solid red;
    text-align: center;
    overflow: hidden;
  }

  .steps-container {
    display: flex;
    transition: transform 0.5s ease-in-out;
  }

  .step-content {
    flex: 0 0 100%;
    width: 20rem;
    height: 30rem;
    border-radius: 8px;
    display: inline-block;
    padding: 20px;
    margin-top: 10px;
    transition: opacity 0.5s ease-in-out;
  }

  .step1 { background-color: #777777; }
  .step2 { background-color: #777777; }
  .step3 { background-color: #777777; }

  .circle-group {
    display: flex;
    gap: 0.5rem;
  }

  .circle {
    width: 5px;
    height: 5px;
    border-radius: 50%;
    background-color: gray;
    display: inline-block;
  }

  .active {
    background-color: black;
  }

  .navigation-buttons {
    margin-top: 10px;
  }

  button {
    color: black;
    background-color: #ebebeb;
    padding: 8px 16px;
    border: none;
    border-radius: 4px;
    margin: 0 5px;
    cursor: pointer;
  }
</style>

<div class="container">
  <div class="steps-container" style={`transform: translateX(-${(currentStep - 1) * 100}%)`}>
    {#each Array(totalSteps) as _, index}
      <div class={`step-content ${currentStep === index + 1 ? 'step' + (index + 1) : ''}`} style={`opacity: ${currentStep === index + 1 ? 1 : 0}`}>
        <p>Contenido del paso {index + 1}</p>
      </div>
    {/each}
  </div>

  <div class="circle-group">
    {#each Array(totalSteps) as _, index}
      <div class={`circle ${currentStep === index + 1 ? 'active' : ''}`}></div>
    {/each}
  </div>

  <div class="navigation-buttons">
    <button on:click={prevStep} disabled={currentStep === 1}>Anterior</button>
    <button on:click={nextStep} disabled={currentStep === totalSteps}>Siguiente</button>
  </div>
</div>













<!-- <script>
  import { onMount } from "svelte";

  const slides = [
    {
      quote: "Kamehameha",
      author: {
        image:
          "http://www.vanguardia.com.mx/sites/default/files/2017118161716.jpg",
        name: "Son Goku",
      },
    },
    {
      quote: "Rasengan",
      author: {
        image:
          "https://vignette.wikia.nocookie.net/naruto/images/3/36/Naruto_Uzumaki.png/revision/latest/scale-to-width-down/300?cb=20170621101149",
        name: "Uzumaki Naruto",
      },
    },
    {
      quote: "Akane, you're my girl!",
      author: {
        image: "https://animeblogjr.files.wordpress.com/2014/02/ranma2.png",
        name: "Saotome Ranma",
      },
    },
    {
      quote: "There is no heaven or hell",
      author: {
        image:
          "http://images6.fanpop.com/image/photos/35600000/L-Lawliet-l-lawliet-35604829-1024-768.jpg",
        name: "L",
      },
    },
  ];

  onMount(() => {
    const radios = document.querySelectorAll("input[type='radio']");
    radios.forEach((radio, index) => {
      radio.addEventListener("change", () => {
        if (radio.checked) {
          let activeSlide = index;
        }
      });
    });
  });
</script>

<div class="carrousel">
  <h2>Coolest anime characters</h2>

  <input type="radio" name="slides" id="radio-1" checked />
  <input type="radio" name="slides" id="radio-2" />
  <input type="radio" name="slides" id="radio-3" />
  <input type="radio" name="slides" id="radio-4" />

  <ul class="slides">
    {#each slides as slide, index}
      <li class="slide" key={index}>
        <p>
          <q>{slide.quote}</q>
          <span class="author">
            <img src={slide.author.image} alt={slide.author.name} />
            {slide.author.name}
          </span>
        </p>
      </li>
    {/each}
  </ul>

  <div class="slidesNavigation">
    {#each slides as slide, index}
      <label for={`radio-${index + 1}`} id={`dotForRadio-${index + 1}`}>
      </label>
    {/each}
  </div>
</div>

<style>
  /*Carousel*/
  .carrousel {
    border: 1px solid red;
    background: #ffffff;
    text-align: center;
    padding: 64px 0;
    height: max-content;
    max-width: 20rem;
    margin: auto;
    position: relative;
    overflow: hidden;
  }
  .carrousel h2 {
    margin: 0;
    margin-top: -27px;
    padding: 0;
    font-size: 16px;
    text-align: center;
    color: #bbbbbb;
  }
  .carrousel .slides {
    width: 400%;
    padding-left: 0;
    padding-top: 16px;
    overflow: hidden;
    list-style: none;
    position: relative;

    -webkit-transition: left 0.75s ease-out;
    -moz-transition: left 0.75s ease-out;
    -o-transition: left 0.75s ease-out;
    transition: left 0.75s ease-out;
  }
  .carrousel .slides li {
    width: 25%;
    position: relative;
    float: left;
  }
  .carrousel li p {
    margin-top: 0;
  }
  .carrousel li q {
    max-width: 90%;
    margin: auto;
    color: #666666;
    font-size: 20px;
    font-weight: bold;
  }
  .carrousel li img {
    width: 90px;
    border-radius: 50%;
    margin-left: -24px;
    margin-right: 8px;
    vertical-align: middle;
  }
  .carrousel li span.author {
    margin-top: 8px;
    font-size: 19px;
    color: #777777;
    display: block;
  }
  .carrousel .slidesNavigation {
    display: block;
    list-style: none;
    text-align: center;
    bottom: 16px;
    /*--- Centering trick---*/
    /* Absolute positioning*/
    position: absolute;
    /* Abosulte positioning*/
    width: 104px; /*This width  is the addition of the width of all the navigations dots - So in this case is   104 because the navigation dots are 26px (width:10px and 6px marginleft + 6 px marginright) and there are 4 dots so 4x26=104 */
    left: 50%; /*Centering de element*/
    margin-left: -52px; /*adjusting the centering by applying a negative margin of half of the width*/
  }
  .carrousel input {
    display: none;
  }
  .carrousel .slidesNavigation label {
    float: left;
    margin: 6px;
    display: block;
    height: 10px;
    width: 10px;
    -webkit-border-radius: 50%;
    border-radius: 50%;
    border: solid 2px #2980b9;
    font-size: 0;
  }
  /* You have to repeat this with each slide */
  #radio-1:checked ~ .slides {
    left: 0;
  }
  #radio-2:checked ~ .slides {
    left: -100%;
  }
  #radio-3:checked ~ .slides {
    left: -200%;
  }
  #radio-4:checked ~ .slides {
    left: -300%;
  }

  .carrousel .slidesNavigation label:hover {
    cursor: pointer;
  }
  /* You have to repeat this with each slide/dot */
  .carrousel #radio-1:checked ~ .slidesNavigation label#dotForRadio-1,
  .carrousel #radio-2:checked ~ .slidesNavigation label#dotForRadio-2,
  .carrousel #radio-3:checked ~ .slidesNavigation label#dotForRadio-3,
  .carrousel #radio-4:checked ~ .slidesNavigation label#dotForRadio-4 {
    background: #2980b9;
  }
</style> -->
