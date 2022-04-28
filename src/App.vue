<template>
  <main class="main-wrapper">
    <!-- <video autoplay></video>
    <button @click="startStream">Camera</button> -->
    <h3>{{ name }}</h3>
    <header-todo />

    <task-input @emit-add-task="addTask" />

    <tab-nav
      :currentView="state.currentView"
      :taskListOverview="taskListOverview"
      @update-current-view="setView"
    />

    <task-list
      :tasksInView="tasksInView"
      @emit-toggle-edit="toggleEdit"
      @emit-delete-task="deleteTask"
    />

    <icon-registry />
  </main>
</template>

<script setup>
import { reactive, computed, onMounted } from "vue";
import { v4 as uuid } from "uuid";
import IconRegistry from "./components/icon/icon-registry.vue";
import HeaderTodo from "./components/HeaderTodo.vue";
import TabNav from "./components/TabNav/TabNav.vue";
import TaskList from "./components/TaskList.vue";
import TaskInput from "./components/TaskInput.vue";
import * as TelegramS from "../scripts/telegram-web-app";

import moment from "moment";
import format from "date-fns/format";

const date = "2021-02-10T19:34:32.227Z";
const currentDateTime = () => {
  return moment(date).format("DD.MM.YYYY");
};
console.log(currentDateTime());

const dateFns = format(new Date(date), "dd.MM.yyyy");
console.log("dateFns ", dateFns);

const state = reactive({
  currentView: "All",
  taskList: [],
  name: "",
  // isPlay: false,
});

const taskLists = reactive({
  all: computed(() => state.taskList),
  current: computed(() => state.taskList.filter((item) => !item.complete)),
  completed: computed(() => state.taskList.filter((item) => item.complete)),
});

const taskListOverview = reactive([
  { name: "All", length: computed(() => taskLists.all.length), status: true },
  {
    name: "Current",
    length: computed(() => taskLists.current.length),
    status: false,
  },
  {
    name: "Completed",
    length: computed(() => taskLists.completed.length),
    status: false,
  },
]);

const tasksInView = computed(() => {
  if (state.currentView === "Current") {
    return taskLists.current;
  } else if (state.currentView === "Completed") {
    return taskLists.completed;
  } else {
    return taskLists.all;
  }
});

const setView = (value) => {
  state.currentView = value;
};

const addTask = (newTaskInput) => {
  state.taskList.push({
    id: uuid(),
    label: newTaskInput,
    complete: false,
    edit: false,
  });
};

const deleteTask = (id) => {
  const index = state.taskList.findIndex((task) => task.id === id);
  state.taskList.splice(index, 1);
};

const toggleEdit = (id) => {
  const index = state.taskList.findIndex((task) => task.id === id);
  state.taskList[index].edit = !state.taskList[index].edit;
};

// const startStream = () => {
//   const video = document.querySelector("video");
//   state.isPlay = !state.isPlay;
//   if (navigator.webkitGetUserMedia != null) {
//     const options = {
//       video: true,
//       audio: false,
//     };

//     navigator.getUserMedia(
//       options,
//       (stream) => {
//         video.srcObject = stream;
//         if (state.isPlay) {
//           video.play();
//         } else {
//           stream.getTracks().forEach(function (track) {
//             track.stop();
//           });
//         }
//       },
//       (err) => {
//         console.log("error happened", err.message);
//       }
//     );
//   }
// };

onMounted(() => {
  let recaptchaScript = document.createElement("script");
  recaptchaScript.setAttribute(
    "src",
    "https://telegram.org/js/telegram-web-app.js"
  );
  document.head.appendChild(recaptchaScript);

  Telegram.WebApp.ready();
  const initData = Telegram.WebApp.initData || "";
  const initDataUnsafe = Telegram.WebApp.initDataUnsafe || {};

  console.log("Telegram.WebApp", Telegram.WebApp);
  console.log("TelegramS.WebApp", TelegramS.WebApp);

  state.name = initData;
});
</script>

<style>
.main-wrapper {
  max-width: 600px;
  margin: 0 auto;
  padding: 0 10px;
}
</style>
