<template>
  <h1 style="text-align: center;">Question Bank</h1>
  <br>
  <br>
  <span v-if='!examButtonVisible'>
  
    <span v-if='questions[counter] && counter != 10'>
    <div style="text-align: center;">Question {{counter+1}}: {{this.questions[counter]['question']}}</div>
    <br>
    <br>
    <div :id="questions[counter]['answer1'] == questions[counter]['correctAnswer']" class="answerBox" @click="adjustCurrentScore">1.) {{this.questions[counter]['answer1']}}</div><br>
    <div :id="questions[counter]['answer2'] == questions[counter]['correctAnswer']" class="answerBox" @click="adjustCurrentScore">2.) {{this.questions[counter]['answer2']}}</div><br>
    <div :id="questions[counter]['answer3'] == questions[counter]['correctAnswer']" class="answerBox" @click="adjustCurrentScore">3.) {{this.questions[counter]['answer3']}}</div><br>
    <div :id="questions[counter]['answer4'] == questions[counter]['correctAnswer']" class="answerBox" @click="adjustCurrentScore">4.) {{this.questions[counter]['answer4']}}</div><br>
    <div :id="questions[counter]['answer5'] == questions[counter]['correctAnswer']" class="answerBox" @click="adjustCurrentScore">5.) {{this.questions[counter]['answer5']}}</div>
    </span>
    <br>

  </span>
  <p style="text-align: center;" v-if='examButtonVisible'>Please make sure the spring boot project is up and running on your localhost before hitting the TAKE EXAM button.</p>
  <p style="text-align: center;" v-if='examButtonVisible'>Once you hit the TAKE EXAM button, a get request is made to my spring boot application which returns an array of json data from the MySQL database.</p>  
  <div class="answerBox" style="text-align: center;" v-if='examButtonVisible' @click="toggleOnExamButton">TAKE EXAM</div>
  <br>
  <h2 style="text-align: center;" v-if='!examButtonVisible && counter != 10'>Your current score is {{currentScore}}/10</h2>
  <h2 style="text-align: center;" v-if='counter == 10'>Your <span style="color:red;">Final score</span> is {{currentScore}}/10</h2>
  <p style="text-align: center;" v-if='counter == 10'>Refresh page to try again</p>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      currentScore: 0,
      examButtonVisible: true,
      endpoint: "http://localhost:8080/questions",
      questions: [],
      randomNumber: 0,
      randomNumberList: [],
      counter: 0,
      finalScore: 0
    }
  },
  methods:{
    toggleOnExamButton(){
      this.examButtonVisible = false;
      this.populateQuestions();

      console.log("reached end");
    },

    adjustCurrentScore(e){
      //console.log(e.target.id);
      if (e.target.id == "true"){
        this.currentScore++;
      }
      this.counter++;
    },

    checkArrayLength(){
      if (this.question.length == 0){
        return false;
      } else return true;
    },

    // generateRandomNumber(){
    //   var num = Math.floor((Math.random() * 20));
    //   this.randomNumber = num;
    // },

    populateQuestions(){
      fetch(this.endpoint)
      .then(res => res.json())
      .then(data => this.questions = data)
      .catch(err => console.log(err.message))
    },

    // nextQuestion(){
    //   //this.wait(1000);
    //   //this.adjustCurrentScore(e);
    //   this.counter++;
    // },

    wait(ms){
      var start = new Date().getTime();
      var end = start;
      while(end < start + ms) {
        end = new Date().getTime();
      }
    }

  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}

.answerBox {
  border-width:2px; 
  border-style:solid; 
  border-color:black; 
  width:500px; 
  margin: 0 auto; 
  padding: 10px 10px;
}
</style>
