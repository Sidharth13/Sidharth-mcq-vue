<template>
  <div id="view-question">
    <ul class="collection with-header">
      <li class="collection-header">
        <h4>{{Questions}}</h4>
      </li>
      <li class="collection-item">Ques No#: {{quesno}}</li>
      <li class="collection-item">OP No 1#: {{op1}}</li>
      <li class="collection-item">OP No 2#: {{op2}}</li>
      <li class="collection-item">OP No 3#: {{op3}}</li>
      <li class="collection-item">Answer: {{ans}}</li>
      <li class="collection-item">Reason: {{res}}</li>
    </ul>
    <router-link to="/" class="btn grey">Back</router-link>
    <button @click="deleteQues" class="btn red">Delete</button>

    <div class="fixed-action-btn">
      <router-link
        v-bind:to="{ name: 'edit-question', params: { quesno: quesno }}"
        class="btn-floating btn-large red"
      >
        <i class="fa fa-pencil"></i>
      </router-link>
    </div>
  </div>
</template>

<script>
import db from "./firebaseinit";
export default {
  name: "view-question",
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
            this.op3s = doc.data().op3;
            this.ans = doc.data().ans;
            this.res = doc.data().res;
          });
        });
    },
    deleteQues() {
      if (confirm("Are you sure?")) {
        db.collection("mcq")
          .where("quesno", "==", this.$route.params.quesno)
          .get()
          .then(querySnapshot => {
            querySnapshot.forEach(doc => {
              doc.ref.delete();
              this.$router.push("/");
            });
          });
      }
    }
  }
};
</script>