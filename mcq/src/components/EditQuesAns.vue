<template>
  <div id="edit-question">
    <div class="row">
      <form @submit.prevent="updateQues" class="col s12">
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="quesno" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="ques" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="op1" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="op2" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="op3" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="ans" required />
          </div>
        </div>
        <div class="row">
          <div class="input-field col s12">
            <input type="text" v-model="res" required />
          </div>
        </div>
        <button type="submit" class="btn">Submit</button>
        <router-link to="/" class="btn grey">Cancel</router-link>
      </form>
    </div>
  </div>
</template>

<script>
import db from "./firebaseinit";
export default {
  name: "edit-question",
  data() {
    return {
      quesno: null,
      ques: null,
      op1: null,
      op2: null,
      op3: null,
      ans: null,
      res: null
    };
  },
  beforeRouteEnter(to, from, next) {
    db.collection("mcq")
      .where("quesno", "==", to.params.quesno)
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          next(vm => {
            vm.quesno = doc.data().quesno;
            vm.ques = doc.data().ques;
            vm.op1 = doc.data().op1;
            vm.op2 = doc.data().op2;
            vm.op3 = doc.data().op3;
            vm.ans = doc.data().ans;
            vm.res = doc.data().res;
          });
        });
      });
  },
  watch: {
    $route: "fetchData"
  },
  methods: {
    fetchData() {
      db.collection("mcq")
        .where("quesno", "==", this.$route.params.quesno)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            this.quesno = doc.data().quesno;
            this.ques = doc.data().ques;
            this.op1 = doc.data().op1;
            this.op2 = doc.data().op2;
            this.op3 = doc.data().op3;
            this.ans = doc.data().ans;
            this.res = doc.data().res;
          });
        });
    },
    updateQues() {
      db.collection("mcq")
        .where("quesno", "==", this.$route.params.quesno)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            doc.ref
              .update({
                quesno: this.quesno,
                ques: this.ques,
                op1: this.op1,
                op2: this.op2,
                op3: this.op3,
                ans: this.ans,
                res: this.res
              })
              .then(() => {
                this.$router.push({
                  name: "view-question",
                  params: { quesno: this.quesno }
                });
              });
          });
        });
    }
  }
};
</script>