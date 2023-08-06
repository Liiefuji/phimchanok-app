<script setup>
import { ref } from 'vue';
import engToThainLanguage from '../src/assets/engToThaiLanguage'

const textarea = ref({
  height: 36,
  minRows: 5,
  curRows: 5,
  textvalue: "",
  textTrans: "",
});

const isCopy = ref(false);
const textinput = ref(null)
const textoutput = ref(null)

const transfromtext = (value) => {
  return value
    .split("")
    .map((item) => {
      return engToThainLanguage[item];
    })
    .join("");
};

const onChangeTextArea = () => {
  const previousRows = textinput.value.rows;
  //รีเซ็ตบรรทัดล่างสุดที่ไม่มีอะไรจนถึง minRows
  textinput.value.rows = textarea.value.minRows;

  const currentRows = Math.trunc(textinput.value.scrollHeight / textarea.value.height);

  if (currentRows === previousRows) {
    textinput.value.rows = currentRows;
  }

  let textTrans = transfromtext(textinput.value.value);

  textarea.value = {
    ...textarea.value,
    curRows: currentRows,
    textvalue: textinput.value.value,
    textTrans,
  }

};

const clickcopy = () => {
  navigator.clipboard.writeText(textarea.value.textTrans);
  isCopy.value = true
  setTimeout(() => {
    isCopy.value = false
  }, 3000);
}

const clickclear = () => {
  textarea.value = {
    ...textarea.value,
    textvalue: "",
    textTrans: "",
    curRows: 5,
  }
}
</script>

<template>
  <div class="App" :style="{ height: '100vh' }">
    <div class="navbar">
      <nav>
        <h3>
          <span>Forgot</span> to Change Language
        </h3>
      </nav>
    </div>
    <div class="Header">
      <h1>
        เปลี่ยนภาษา <span>อังกฤษ</span> ให้เป็นภาษา <span>ไทย</span>
      </h1>
    </div>
    <div class="MainInputDiv " :style="{ lineHeight: textarea.height }">
      <!-- <MdContentCopy size="1.8em" class="copyicon" :class="{ 'displaynone': textarea.textvalue }" @click="clickcopy" />
      <MdClear :class="{ 'displaynone': textarea.textvalue }" size="2em" class="clearicon" :onClick="clickclear" /> -->
      <textarea @input="onChangeTextArea($event)" :value="textarea.textvalue" :rows="textarea.curRows" class="textinput"
        ref="textinput" placeholder="Enter your text" />
      <textarea readOnly :value="textarea.textTrans" @change="onChangeTextArea" :rows="textarea.curRows" ref="textoutput"
        class="textoutput" />
    </div>
    <div class="Snackbar" :class="{ 'show': isCopy }">
      <p>Copied to clipboard</p>
    </div>
  </div>
</template>

<style scoped lang="scss">
.navbar {
  font-family: "Montserrat", sans-serif;
  box-shadow: 0px 0px 10px rgb(0, 0, 0, 0.2);
  font-size: 18px;
  height: 64px;
  width: 100%;
  background-color: #f5f5f5;
  position: fixed;
  display: flex;
  align-items: center;
  justify-content: flex-start;

  nav {
    width: 90%;
    margin: 0 auto;

    span {
      color: #a30000;
    }
  }
}

@keyframes fadein {
  from {
    bottom: 0px;
    opacity: 0;
  }

  to {
    bottom: 30px;
    opacity: 1;
  }
}

@keyframes fadeout {
  from {
    bottom: 30px;
    opacity: 1;
  }

  to {
    bottom: -30px;
    opacity: 0;
  }
}

.Snackbar {
  visibility: hidden;
  height: 48px;
  width: 276px;
  font-size: 17px;
  position: fixed;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1;
  right: 30px;
  bottom: 0px;
  border: 1px solid #ccc;
  border-left: 8px brown solid;
  border-radius: 10px;
  box-shadow: 2px 2px 10px rgb(0, 0, 0, 0.2);

  &.show {
    visibility: visible;
    animation: fadein 0.5s forwards,
      fadeout 0.5s forwards 2s;
  }
}

.Header {
  font-family: "Prompt", sans-serif;
  height: auto;
  min-height: 226px;
  width: 100%;
  margin: 0px auto 30px auto;
  padding-top: 94px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: #fff;

  div {
    height: 42px;
    width: 70%;
    margin-top: 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  h1 {
    font-size: 40px;

    span {
      display: inline-block;
      cursor: pointer;
      color: #a30000;

      :after {
        display: block;
        content: "";
        border-bottom: solid 3px #a30000;
        transform: scaleX(0);
        transform-origin: left;
        transition: transform 250ms ease-in-out;
      }

      :hover {
        :after {
          transform: scaleX(1);
        }
      }
    }
  }
}

.MainInputDiv {
  height: auto;
  width: 70%;
  margin: 0px auto 25px auto;
  position: relative;
  display: flex;
  align-items: flex-start;
  justify-content: center;

  textarea {
    max-height: 700px;
    font-family: "Prompt", sans-serif;
    font-size: 24px;
    padding: 15px 92px 15px 15px;
    width: 100%;
    outline: none;
    resize: none;
    border: 1px solid #ccc;
    border-radius: 10px 0 0 10px;
    box-shadow: 2px 2px 10px rgb(0, 0, 0, 0.2);
  }

  .copyicon {
    position: absolute;
    right: 2%;
    top: 20px;
    color: #919191;
    cursor: pointer;

    :hover {
      color: #a30000;
    }
  }

  .clearicon {
    position: absolute;
    left: 46%;
    top: 16px;
    color: #919191;
    cursor: pointer;

    :hover {
      color: #a30000;
    }
  }
}
</style>
