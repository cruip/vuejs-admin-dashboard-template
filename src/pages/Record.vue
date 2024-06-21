<template>
  <div>
    <header>
      <div class="search-container">
        <div class="logo">LOGO</div>
        <div class="search-content">
          <div class="search">
            <input
              type="text"
              placeholder="Search Recording"
              class="search-input"
            />
            <button class="search-button">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                width="24"
                height="24"
              >
                <path
                  d="M23.384,20.616l-5.094-5.094c1.172-1.531,1.881-3.438,1.881-5.484C20.172,4.455,15.717,0,10.086,0C4.455,0,0,4.455,0,10.086c0,5.63,4.455,10.085,10.086,10.085c2.046,0,3.953-0.709,5.484-1.881l5.094,5.094c0.472,0.472,1.103,0.736,1.736,0.736c0.632,0,1.264-0.264,1.736-0.736C24.329,23.144,24.329,21.56,23.384,20.616z M10.086,18.172c-4.472,0-8.086-3.613-8.086-8.086c0-4.472,3.613-8.086,8.086-8.086c4.472,0,8.086,3.613,8.086,8.086C18.172,14.558,14.558,18.172,10.086,18.172z"
                />
              </svg>
            </button>
          </div>
        </div>
      </div>
    </header>
    <div class="record-apge">
      <div class="record-apge__header">
        your Recording <button>Add script</button>
      </div>
      <div class="recording-container">
        <div class="recording-content">
          <button
            v-if="!isRecord"
            class="recording-button"
            @click="startRecording"
          >
            <div class="bg-icon"><mid-icon /></div>
          </button>
          <div v-else class="recording-content__stop">
            <button class="recording-button" @click="pauseResumeRecording">
              <div v-if="!isPause" class="bg-icon"><vector-icon /></div>
              <div v-else class="bg-icon"><pause-icon /></div>
            </button>
            <button class="recording-button" @click="stopRecording">
              <div class="bg-icon"><stop-icon /></div>
            </button>
          </div>
          <h3 v-if="!isRecord">Start Recording</h3>
          <h3 v-else>Recording ...</h3>
        </div>
      </div>
      <div class="separator">
        <span class="line"></span>
        <span class="or-text">OR</span>
        <span class="line"></span>
      </div>
      <div class="recording-container">
        <div class="recording-content">
          <button class="recording-button" @click="uploadFile">
            <div class="bg-icon"><upload-file-icon /></div>
          </button>
          <h3>Upload file</h3>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import RecordRTC from "recordrtc";
import { ref } from "vue";
import MidIcon from "../icons/Mid.vue";
import UploadFileIcon from "../icons/Upload.vue";
import VectorIcon from "../icons/Vector.vue";
import StopIcon from "../icons/Stop.vue";
import PauseIcon from "../icons/Pause.vue";
let isRecord = ref(false);
let isPause = ref(false);
let recorder = ref(null);
let blobFile = ref(null);

function startRecording() {
  isRecord.value = true;
  navigator.mediaDevices
    .getUserMedia({ audio: true, video: false })
    .then((stream) => {
      recorder.value = RecordRTC(stream, {
        type: "audio",
        mimeType: "audio/wav",
      });
      recorder.value.startRecording();
    })
    .catch((error) => {
      console.error("Could not start recording: ", error);
    });
}

function pauseResumeRecording() {
  if (isRecord.value) {
    if (recorder.value.getState() === "recording") {
      recorder.value.pauseRecording();
      isPause.value = true;
    } else {
      recorder.value.resumeRecording();
      isPause.value = false;
    }
  }
}

function stopRecording() {
  if (isRecord.value) {
    recorder.value.stopRecording(() => {
      blobFile.value = recorder.value.getBlob();
      console.log(blobFile.value);
      isRecord.value = false;
      // Do something with the recorded blob...
    });
  }
}

function uploadFile() {
  // Do something with the uploaded file...
  console.log("Upload file", blobFile.value);
}

function downloadBlob(blob, filename) {
  // Create an object URL for the blob
  const url = window.URL.createObjectURL(blob);

  // Create a new 'a' element
  const a = document.createElement("a");

  // Set the href and download for the 'a' element
  a.href = url;
  a.download = filename || "download";

  // Append the 'a' element to the body and click it
  document.body.appendChild(a);
  a.click();

  // After a delay, remove the element and revoke the object URL
  setTimeout(() => {
    document.body.removeChild(a);
    window.URL.revokeObjectURL(url);
  }, 60);
}
</script>
<style scoped>
.search-container {
  display: flex;
  align-items: center;
  background-color: #e0e7f1;
  padding: 10px 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
.search-content {
  flex-grow: 1;
  display: flex;
  justify-content: center;
}

.search {
  background-color: #d3e0f4;
  display: flex;
  width: 500px;
  align-items: center;
  padding: 5px 20px;
  border-radius: 20px;
}
.logo {
  margin-right: 20px;
  font-weight: bold;
  font-size: 18px;
  color: #333;
}

.search-input {
  border: none;
  padding: 10px;
  border-radius: 20px;
  outline: none;
  font-size: 16px;
  flex-grow: 1;
  background-color: transparent !important;
}
.search-input:focus {
  outline: none !important;
  --tw-ring-color: transparent !important;
}

.search-button {
  background: none;
  border: none;
  padding: 0;
  margin-left: 10px;
  cursor: pointer;
  outline: none;
}

.search-button svg {
  fill: #666;
}

.search-button svg:hover {
  fill: #333;
}

.record-apge {
  padding: 30px 60px;
}
.record-apge__header {
  text-align: left;
  font-size: 24px;
  font-weight: 600;
  line-height: 36px;
  letter-spacing: -0.01em;
  text-align: left;
  text-transform: capitalize;
}
.record-apge__header button {
  border-radius: 5px;
  padding: 7px 25px;
  background-color: #6366f1;
  color: #fff;
  outline: none;
  border: none;
  display: inline-flex;
  align-items: center;
  gap: 6px;
  font-size: 20px;
  line-height: 32px;
  font-weight: 400;
  margin-left: 20px;
}
.recording-container {
  border: 2px dashed #6366f1;
  border-radius: 8px;
  padding: 30px;
  background-color: #f1f7ff;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 30px 0;
}

.recording-content h3 {
  font-weight: 500;
  font-size: 32px;
  line-height: 28px;
}

.recording-content__stop {
  display: flex;
  align-items: center;
  gap: 20px;
}
.recording-button {
  background-color: transparent;
  border: none;
  color: #6666ff;
  font-size: 16px;
  align-self: center;
  display: flex;
  align-items: center;
  cursor: pointer;
  outline: none;
  margin: 0 auto 30px;
}

.bg-icon {
  background-color: #cbdef9;
  height: 100px;
  width: 100px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.recording-button:hover {
  color: #5555ff;
}

.separator {
  display: flex;
  align-items: center;
  width: 100%;
  margin: 40px 0;
}

.line {
  flex-grow: 1;
  border-bottom: 2px dashed #5555ff;
  margin: 0 10px;
}

.or-text {
  white-space: nowrap;
  color: #000000;
  font-weight: bold;
  font-size: 31px;
  line-height: 46px;
}
</style>
