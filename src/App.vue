<script setup lang="ts">
import { copy } from "clipboard";
import { ref } from "vue";
import Button from "./components/Button.vue";
import { escapeHTML } from "./utils/escapeHTML";

const editorRef = ref<HTMLDivElement>();

function undo() {
  document.execCommand("undo", false);
}

function redo() {
  document.execCommand("redo", false);
}

function makeHeading() {
  document.execCommand("formatBlock", false, "<H2>");
}

function makeParagraph() {
  document.execCommand("formatBlock", false, "<P>");
}

function insertImage() {
  const url = prompt("Введите URL изображения");

  if (url) {
    document.execCommand("insertImage", false, url);
  }
}

function copyHTML() {
  const target = editorRef.value;

  if (!target) return;

  const result: string[] = [];

  for (const child of target.children) {
    if (child instanceof HTMLHeadingElement) {
      result.push(`<h2>${escapeHTML(child.innerText)}</h2>`);
    } else if (child instanceof HTMLParagraphElement) {
      result.push(`<p>${escapeHTML(child.innerText)}</p>`);
    } else if (child instanceof HTMLImageElement) {
      result.push(`<img src="${child.src}" />`);
    }
  }

  copy(result.join("\n"));
}

function handlePaste(e: ClipboardEvent) {
  e.preventDefault();

  const data = e.clipboardData?.getData("text/plain");

  if (data) {
    document.execCommand("inserttext", false, data);
  }
}
</script>

<template>
  <div class="flex flex-col gap-8 p-5 sm:p-8 md:p-28">
    <div class="flex gap-3">
      <Button title="Отменить" @click="undo">
        <i class="fas fa-rotate-left fa-lg"></i>
      </Button>

      <Button title="Повторить" @click="redo">
        <i class="fas fa-rotate-right fa-lg"></i>
      </Button>

      <Button title="Заголовок" @click="makeHeading">
        <i class="fas fa-heading fa-lg"></i>
      </Button>

      <Button title="Абзац" @click="makeParagraph">
        <i class="fas fa-paragraph fa-lg"></i>
      </Button>

      <Button title="Вставить изображение" @click="insertImage">
        <i class="fas fa-image fa-lg"></i>
      </Button>

      <Button flat @click="copyHTML">Скопировать HTML</Button>
    </div>

    <div ref="editorRef" contenteditable @paste="handlePaste">
      <p>
        Таким образом консультация с широким активом представляет собой
        интересный эксперимент проверки позиций, занимаемых участниками в
        отношении поставленных задач. С другой стороны постоянное
        информационно-пропагандистское обеспечение нашей деятельности
        представляет собой интересный эксперимент проверки форм развития.
        Идейные соображения высшего порядка, а также укрепление и развитие
        структуры влечет за собой процесс внедрения и модернизации
        соответствующий условий активизации. Задача организации, в особенности
        же реализация намеченных плановых заданий играет важную роль в
        формировании дальнейших направлений развития. Повседневная практика
        показывает, что постоянное информационно-пропагандистское обеспечение
        нашей деятельности играет важную роль в формировании существенных
        финансовых и административных условий.
      </p>

      <h2>Смотрите какие обезьянки</h2>

      <img
        src="https://s3-alpha-sig.figma.com/img/41f2/0008/69cac4e6271b50281c7941f0ba76b675?Expires=1696809600&Signature=I7gq6N3DoeK7kye8SH1Hdm-XSRDT6PLM5ca-ls2o9OUi3Lp7hmzsae7UPaSk2bcNqpR5gJO~XuHC3MIF7lnwQsG5xbLwGbv-VDA9XyzpmCjwcSDjs2mh4J2rmd4vusu7sPspo3xNlpHx5kxsUGBCuuBwGXKAL-11czUaNnoj00epP4~1bTpxttTVLWaoAA7oQ~avTn~YQSbbmTZ79c~SO~4bOtGH2GTgdCcT-SDEcuDaP4SvfpyHUfvg52AaIzndGGr5EQtVtVvJM6m-P7z2KWtandPc8QVaRY4aX~83vVSOJgIPp9nHdSPUEAhzMTeVaA5hewD~wS7XdPJVdPPNPw__&Key-Pair-Id=APKAQ4GOSFWCVNEHN3O4"
      />

      <p>
        Таким образом консультация с широким активом представляет собой
        интересный эксперимент проверки позиций, занимаемых участниками в
        отношении поставленных задач. С другой стороны постоянное
        информационно-пропагандистское обеспечение нашей деятельности
        представляет собой инйцу шо шщйоц ущойц ущошцщйуо йцщуо йщцоу щйоу
        шщйцош ущйтересный эксперимент проверки форм развития. Идейные
        соображения высшего порядка, а также укрепление и развитие структуры
        влечет за собой процесс внедрения и модернизации соответствующий условий
        активизации. Задача организации, в особенности же реализация намеченных
        плановых заданий играет важную роль в формировании дальнейших
        направлений развития. Повседневная практика показывает, что постоянное
        информационно-пропагандистское обеспечение нашей деятельности играет
        важную роль в формировании существенных финансовых и административных
        условий.
      </p>

      <p>
        Товарищи! новая модель организационной деятельности требуют от нас
        анализа направлений прогрессивного развития. Задача организации, в
        особенности же постоянный количественный рост и сфера нашей активности
        требуют от нас анализа позиций, занимаемых участниками в отношении
        поставленных задач. Задача организации, в особенности же реализация
        намеченных плановых заданий требуют от нас анализа системы обучения
        кадров, соответствует насущным потребностям.
      </p>
    </div>
  </div>
</template>
