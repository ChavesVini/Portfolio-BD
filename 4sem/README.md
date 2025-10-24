<h1 align="center">API 4º Semestre - 02/2024</h1>
<h1 align="center"> 
  <a href="https://github.com/manolito-fatec/geo-iot-2024-1"><img src="https://img.shields.io/badge/GitHub-Repositório Projeto-181717?style=for-the-badge&logo=github"></a>
</h1>

# Parceiro - [ITO1 Technology](https://www.ito1.com.br/)
![images](https://github.com/user-attachments/assets/c2b2827f-883a-4889-98d8-6e74f3db2456)

# Resumo do projeto
O projeto tem como objetivo desenvolver uma solução robusta para o armazenamento e consulta de dados de geolocalização em tempo real, gerados por dispositivos IoT, como wearables, tags e smartphones. Utilizando um banco de dados SQL, ORACLE CLOUD, a solução será capaz de lidar com grandes volumes de dados, garantindo escalabilidade e alta disponibilidade. A arquitetura proposta envolve o uso de Spring Boot para o backend, Vue.js para o frontend e comunicação eficiente via APIs RESTful e WebSocket. A segurança dos dados será garantida com criptografia e controle de acesso.

# Tecnologias
<img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/java/java-original.svg" width="100" height="100"/> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/spring/spring-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/git/git-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/oracle/oracle-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/docker/docker-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vuejs/vuejs-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/vitejs/vitejs-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/redis/redis-original.svg" width="100" height="100" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon@latest/icons/typescript/typescript-original.svg" width="100" height="100" />

- Java: Linguagem usada no back-end;
- Spring Boot: Framework usado para criação das classes e os links para o front-end e o banco de dados;
- Git: Ferramenta para versionamento de controle de versão;
- Oracle: Banco de dados utilizado para esse projeto;
- Docker: Plataforma para containerização de aplicações;
- Vue.js: Framework no front-end para fazer as telas da aplicação;
- Vite.js: Framework utilizado para builds e o desenvolvimento;
- Redis: Ferramenta para cache;
- TypeScript: Linguagem usada no front-end.

# Contribuições Individuais
No quarto semestre, contribui no front-end e no back-end.

Criei a lógica e o estilo da tela de registro de usuário.

<details>
  <summary> Código em TypeScript - Registro de usuário </summary>
    
```typescript
<script setup>
import router from '@/router'
import {ref} from 'vue';
import Logo from "@/assets/Logo.png";
import {useToast} from "vue-toastification";
import {darkModeClick} from '@/components/stores/StoreDarkModeGetClick.js';
import {registerUser, verifyIfHaveTwoEmails} from "@/services/apiService.ts";

const toast = useToast();
const email = ref("");
const password = ref("");
const confirmPassword = ref("")
const option = ref("");
const errorEmail = ref(false);
const errorPassword = ref(false);
const errorConfirmPassword = ref(false);
const errorOption = ref(false);
const store = darkModeClick();

const returnHome = () => {
  router.push("/home");
}

async function submitUser() {
  const response = await verifyIfHaveTwoEmails(email.value);

  errorEmail.value = false;
  errorPassword.value = false;
  errorConfirmPassword.value = false;
  errorOption.value = false;

  if (!email.value) {
    errorEmail.value = true;
  }
  if (!password.value) {
    errorPassword.value = true;
  }
  if (!confirmPassword.value) {
    errorConfirmPassword.value = true;
  }
  if (!option.value) {
    errorOption.value = true;
  } else if (!(email.value.match("@"))) {
    toast.error("Coloque um e-mail válido!")
  } else if (password.value != confirmPassword.value) {
    toast.error("As senhas não conferem.");
  } else if (response.emailExist) {
    toast.error("O usuário já está cadastrado.");
  } else {
    await registerUser(email.value,
        password.value,
        option.value
    )
    toast.success("Usuário Cadastrado!");

    email.value = "";
    password.value = "";
    confirmPassword.value = "";
    option.value = "";
  }
}
  
```
</details> 

Atuei também para criar a lógica do modo escuro e modo claro da aplicação.

<details>
  <summary> Código em TypeScript - Modo escuro e modo claro </summary>
  
Estilo:
```typescript
<template>  
  <div class="dark-white-mode">
    <input type="checkbox" id="toggle" @click="$emit('toggleDarkLightMode')"/>
    <label for="toggle" class="toggle-label">
      <i class="fas fa-moon moon"></i>
      <i class="fas fa-sun sun"></i>
    </label>
  </div>
</template>

<style scoped>

input[type="checkbox"] {
  display: none;
}

.toggle-label {
  cursor: pointer;
  display: flex;
  position: relative;
  justify-content: center;
  align-items: center; 
  width: 35px;
  height: 35px;
}

.sun, .moon {
  text-align: center;
  position: absolute;
  font-size: 35px;
  transition: 0.3s;
}

.sun {
  opacity: 0;
  color: white;
}

.moon {
  opacity: 1;
  color: black;
}

input[type="checkbox"]:checked + .toggle-label .sun {
  opacity: 1;
}

input[type="checkbox"]:checked + .toggle-label .moon {
  opacity: 0;
}

</style>
```

Lógica:
```typescript
let darkOrWhiteMap: string;
const baseLayer = ref<BaseLayer>();
const mapMode = ref(false);

function toggleTheme() {
  mapMode.value = !mapMode.value;

  if (mapMode.value) {
    darkOrWhiteMap = 'streets-v2-dark';
  } else {
    darkOrWhiteMap = 'streets-v2';
  }

  baseLayer.value?.setSource(
    new XYZ({
      url: `https://api.maptiler.com/maps/${darkOrWhiteMap}/{z}/{x}/{y}.png?key=eR9oB64MlktZG90QwIJ7`
    })
  );
}
```

</details>

Atuei com uma das partes mais importantes do projeto, que foi o playback do icone, quando ele for mexendo na tela, além de poder escolher qualquer momento para avançar ou retroceder e também controlar as velocidades

<details>
  <summary>Código em TypeScript - Playback </summary>
  
```javascript

import 'ol/ol.css';
import { getClick } from '@/components/stores/StoreGetClick.js' 
import DropdownSpeed from '@/components/filter/DropdownSpeed.vue';
import ButtonStartPause from '@/components/ButtonStartPause.vue';
import { ref, defineProps, watch } from 'vue';
import Feature from 'ol/Feature';
import { LineString } from 'ol/geom';
import ButtonBackward from '@/components/ButtonBackward.vue';
import ButtonForward from '@/components/ButtonForward.vue';
import ButtonRestart from '@/components/ButtonRestart.vue';
import { Icon, Style } from 'ol/style';
import IconPositionMap from '../assets/IconPositionMap.png';
import Coordinate from 'ol/coordinate';
import { onMounted } from 'vue';

const store = getClick();
const animating = ref(false);
const startTime = ref(0);
let angulo = 0;

const elapsedTime = ref(0);
const duration = ref(30000);
const selectedValue = ref('1x');

const props = defineProps<{
  rota: LineString,
  iconMap: Feature,
  allCoordinatesAnimation: Coordinate[],
  anguloInicial: number,
}>();

function watchChanges(newValue) {
  const playbackControl = document.getElementById('control-movement');
  const buttonStartPause = document.getElementById('button-start-pause');

  if (newValue) {
    playbackControl.style.width = '59.78%';
    buttonStartPause.style.left = '550px';
    playbackControl.style.left = '520px';
  } else {
    playbackControl.style.width = '92.6%';
    buttonStartPause.style.left = '145px';
    playbackControl.style.left = '100px';
  }
}

watch(
  () => store.onClickFilters,
  (newValue) => {
    watchChanges(newValue);
  }
);

function getRotationIcon() {
  const coordinates = props.rota.getCoordinates();
  const progress = Math.min(elapsedTime.value / duration.value, 1);

  let currentIndex = 0;
  let accumulatedDistance = 0;

  for (let i = 0; i < coordinates.length - 1; i++) {

    const coord1 = coordinates[i];
    const coord2 = coordinates[i + 1];

    const dist = Math.sqrt(Math.pow(coord2[0] - coord1[0], 2) + Math.pow(coord2[1] - coord1[1], 2));
    accumulatedDistance += dist;

    if (accumulatedDistance >= props.rota.getLength() * progress) {
      currentIndex = i;
      break;
    }
  }

  const currentCoord = coordinates[currentIndex];
  const nextCoord = coordinates[Math.min(currentIndex + 1, coordinates.length - 1)];

  const deltaLon = nextCoord[0] - currentCoord[0];
  const deltaLat = nextCoord[1] - currentCoord[1];

  angulo = Math.atan2(deltaLat, deltaLon) * -1;
}

function handleChangeColorRange() {
  if (!animating.value) {
    animating.value = false;
    startTime.value = new Date().getTime() - elapsedTime.value;

    const rangeInput = document.getElementById('speed');
    const percentage = ((elapsedTime.value / duration.value) * 100) + 0.1;
    
    rangeInput.style.setProperty('--elapsedTime', `${percentage}%`);
    
    const progress = Math.min(elapsedTime.value / duration.value, 1); 
    const coord = props.rota.getCoordinateAt(progress);
    props.iconMap.getGeometry().setCoordinates(coord);

  } else {
    
    animating.value = false;
    startTime.value = new Date().getTime() - elapsedTime.value;

    const rangeInput = document.getElementById('speed');
    const percentage = ((elapsedTime.value / duration.value) * 100) + 0.1;
    
    rangeInput.style.setProperty('--elapsedTime', `${percentage}%`);
    
    const progress = Math.min(elapsedTime.value / duration.value, 1); 
    const coord = props.rota.getCoordinateAt(progress);
    props.iconMap.getGeometry().setCoordinates(coord);

    continueAnimation();
  }
}

function changeColorRange() {
  const rangeInput = document.getElementById('speed');
  const percentage = ((elapsedTime.value / duration.value) * 100) + 0.1;
  rangeInput.style.setProperty('--elapsedTime', `${percentage}%`);
} 

function continueAnimation() {
  if (!animating.value) {
    initiateAnimation();
  } 
}

function typeVelocity() {
  if (selectedValue.value === "0.5x") {
    if (!animating.value) {
      adjustVelocity(0.5);
    } else {
      pauseAnimation();
      adjustVelocity(0.5);
      continueAnimation();
    }
  } else if (selectedValue.value === "1x") {
    if (!animating.value) {
      adjustVelocity(1);
    } else {
      pauseAnimation();
      adjustVelocity(1);
      continueAnimation();
    }
  } else {
    if (!animating.value) {
      adjustVelocity(2);
    } else {
      pauseAnimation();
      adjustVelocity(2);
      continueAnimation();
    }
  }
}

function adjustVelocity(duracao) {
  if (!animating.value) {
    const progress = elapsedTime.value / duration.value;
    duration.value = 30000 / duracao;
    elapsedTime.value = progress * duration.value;
  } else {
    pauseAnimation();
    const progress = elapsedTime.value / duration.value;
    duration.value = 30000 / duracao;
    elapsedTime.value = progress * duration.value;
    continueAnimation();
  }
  adjustPosition();
}

function startAndPause() {
  if (animating.value) {
    pauseAnimation();
  } else {
    initiateAnimation();
  }
}

function initiateAnimation() {
  animating.value = true;
  startTime.value = new Date().getTime() - elapsedTime.value;
  requestAnimationFrame(routeAnimation); 
}

function pauseAnimation() {
  if (animating.value) {
    animating.value = false;
    elapsedTime.value = (new Date().getTime() - startTime.value);
  }
}

function routeAnimation() {
  if (animating.value) {
    const progress = Math.min(elapsedTime.value / duration.value, 1); 
    elapsedTime.value = new Date().getTime() - startTime.value;

    const coord = props.rota.getCoordinateAt(progress);
    props.iconMap.getGeometry().setCoordinates(coord);

    changeColorRange();
    getRotationIcon();

    props.iconMap.setStyle(new Style({
      image: new Icon({
        src: IconPositionMap,
        anchor: [0.5, 0.5],
        scale: 0.2,
        rotation: angulo
      }),
    }));

    if (progress < 1) {
      requestAnimationFrame(routeAnimation);
    }
  }
}

function adjustPosition() {
  const progress = Math.min(elapsedTime.value / duration.value, 1);
  const coord = props.rota.getCoordinateAt(progress);
  props.iconMap.getGeometry().setCoordinates(coord);
  elapsedTime.value = progress * duration.value;
}

function restartAnimation() {
  elapsedTime.value = 0;

  changeColorRange();

  props.iconMap.setStyle(new Style({
    image: new Icon({
      src: IconPositionMap,
      anchor: [0.5, 0.5],
      scale: 0.2,
      rotation: props.anguloInicial
    }),
  }));

  adjustPosition();
  
  if (animating.value) {
    initiateAnimation();
  }
}

onMounted(() => {
  watchChanges(store.onClickFilters);
})

</script>
```

</details>


# Aprendizados Efetivos
Foi a minha primeira vez pegando front-end para desenvolver, me acostumei rápido por conta da sintaxe ser bem mais fácil. Além disso, gostei de ficar apenas no front-end esse semestre pois tinha um desafio novo a minha frente, e saber que eu consegui aprender o suficiente para entender a sintaxe e ainda sim fazer os códigos foi muito gratificante.

## Hard Skills
<details>
  
| Habilidade | Nota | Classificação |
| :-----: | :-----: | :-----: |
| GitHub |	★★★★★ | Sei fazer com autonomia |
| TypeScript | ★★☆☆☆ | Já ouvi falar |
| JavaScript | ★★☆☆☆ | Já ouvi falar |
| Vue.js | ★★☆☆☆ | Já ouvi falar |

</details> 

## Soft Skills
<details>
  
| Habilidade |	Classificação |
| :-----: | :-----: |
| Trabalho em equipe |	Busquei ajudar o time a entender e corrigir bugs que vinham do processo |
| Colaboração | Sempre disponivel, ajudava quem precisasse de ajuda |
| Autonomia | Fui muito autonomo, raramente pedia ajuda externa ou de outros grupos |

</details> 

<h1 align="center">Outros Projetos</h1>

- [1º Semestre - Avaliação 360º](../1sem/README.md)
- [2º Semestre - TG Manager](../2sem/README.md)
- [3º Semestre - Dom Rock Pipeline Configurator](../3sem/README.md)
- 4º Semestre - GEO-IOT
- [5º Semestre - VISION](../5sem/README.md)
- [6º Semestre - LuminIA](../6sem/README.md)


<p align="center"><a href="../README.md">Voltar a página inicial</p>


