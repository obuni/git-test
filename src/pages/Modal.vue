<template>
    <div class="modal" :class="{'is-active': modalOpen}">
    <div class="modal-background" @click="closeModal"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">날짜</p>
          <button class="delete" aria-label="close"></button>
        </header>
        <section class="modal-card-body">
            <textarea
                  class="textarea"
                  :placeholder="placeholder"
                  @input="typing"
                  :value="message"
             ></textarea>
        </section>
        <footer class="modal-card-foot">
            <button class="button is-primary" @click="save">
            <!-- 저장 클릭 이벤트 -->
            {{ buttonMessage[0] }}
            </button>
            <button class="button" @click="closeModal">{{ buttonMessage[1] }}</button>
        </footer>
      </div>
    </div>
  </template>
  
  <script>
    export default {
        name: "Modal",
    props: ["modalOpen"],
    data() {
      return {
        placeholder: "오늘 할 일을 적어주세요",
        buttonMessage: ["저장", "취소"],
      };
    },
    methods: {
    typing(event) {
        this.message = event.target.value;
      },
      closeModal() {
        this.$emit("closeModal", false); // $emit('이벤트명', value)
      },
      save() {
        axios.post('http://localhost:4000/save', {
          message: this.message,
          date: `${this.year}-${this.month}-${this.clickDay}`,
        }).then((result) => {
          console.log('result', result.data);
        }).catch((e) => {
          console.log('e', e);
        });
      },
    },
    };
  </script>