<template>
  <div class="game-container" v-if="!errorPage">
    <div class="game-form" v-if="!noMoreCountry">
      <div class="game-step" v-if="step1">
        <h1 class="game-title">Welcome, Sam!</h1>
        <div class="game-subtitle margin-styles">
          You will be given a country and you have to guess the capital city of
          that country. Best of luck!
        </div>
        <button class="game-button start-button" @click="startGame">Start the Game</button>
      </div>
      <div class="game-step" v-else-if="step2">
        <h1 class="game-title">
          What is the capital city of {{ country.name }}?
        </h1>
        <div class="game-input-container">
          <input
            type="text"
            id="capital"
            v-model="capital"
            placeholder="Type your answer here"
          />
          <button class="game-submit-button" @click="submitAnswer(capital)">
            Submit
          </button>
        </div>
        <div class="game-button-container">
          <button class="game-button" @click="selectRandomCountry">
            Don't know? Change the country
          </button>
        </div>
      </div>
      <div class="game-step" v-else>
        <h1 class="game-title">
          <p v-if="correctAnswer">Your answer is <span class="green">correct</span>!</p>
          <p v-else>Your answer is <span class="red">incorrect</span>.</p>
        </h1>
        <div class="game-subtitle">
          The capital of {{ country.name }} is <span class="bold">{{ country.capital }}</span> 
        </div>
        <br>
        <button class="game-button" @click="reset">Play Again</button>
      </div>
    </div>
    <div class="game-form" v-else>
      <h1 class="game-title">No more countries</h1>
      <button class="game-button" @click="reset">Start Again</button>
    </div>
  </div>
  <div class="game-container" v-else>
    <div class="game-form">
       <h1 class="game-title">{{ errorMesage }}</h1>
    </div>
  </div>
</template>



<script>
export default {
    name: 'Home',
    data(){
     return {
        step1: true,
        step2: false,
        countries: [],
        countriesClone: [],
        noMoreCountry: false,
        country: '',
        capital: '',
        correctAnswer: '',
        errorPage: false,
        errorMesage: ''
    }
    },
    mounted () {
        this.apiCall()
    },
    methods: {
        startGame () {
            this.step1 = false
            this.step2 = true
            this.selectRandomCountry()
        },
        reset () {
            this.step1 = true
            this.step2 = false
            if (this.noMoreCountry) this.countries = this.countriesClone
        },
        submitAnswer(capital) {
            this.step2 = false
            if (this.country.capital.toLowerCase() === capital.toLowerCase()) {
                this.correctAnswer = true
                return
            }
            this.correctAnswer = false
            this.capital = ''
        },
        selectRandomCountry() {
            const random = Math.floor(Math.random() * this.countries.length)
            this.country = this.countries[random]
            this.countries.splice(random,1)
            if (this.countries.length === 0) this.noMoreCountry = true
        },
        async apiCall () {
          try {
             const resp = await fetch('https://countriesnow.space/api/v0.1/countries/capital')
            const json =  await resp.json()
            if (json.error) {
              this.errorPage = true
              this.errorMesage = json.msg
            }
            this.countries = json.data
            this.countriesClone = json.data
            
          } catch (error) {
             this.errorPage = true
            this.errorMesage = `We are facing some technical difficulties. Please try again later. (${error.message})`
          }
        }
    }
}
</script>


 <style scoped>
.game-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-color: #f2f2f2;
background: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('~@/assets/Flags-world-flags-Country-history-blog-travel-2009.webp');
background-size: cover;

}

.game-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
}

.game-step {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.bold {
  font-weight: bold;
}
.margin-styles {
  margin-top: 20px;
  margin-bottom: 20px
}
.game-title {
  font-size: 32px;
  font-weight: bold;
  margin-bottom: 20px;
  color: #333;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.game-subtitle {
  font-size: 24px;
  margin-bottom: 30px;
  color: #555;
  text-align: center;
}

.game-input-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  margin-bottom: 50px;
  margin-top: 20px;
}

#capital {
  flex: 1;
  margin-right: 10px;
  width: 275px;
  padding: 15px;
  font-size: 18px;
  border-radius: 4px;
  border: none;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
}

.game-submit-button {
  flex: 0 0 auto;
  font-size: 18px;
  background-color: #007bff;
  color: #fff;
  padding: 15px 35px;
  border: none;
  border-radius: 4px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
  cursor: pointer;
}

.game-submit-button:hover {
  background-color: #0056b3;
}

.game-button-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-bottom: 30px;
}
.start-button {
  margin-top: 20px;

}
.game-button {
  font-size: 18px;
  background-color: #6c757d;
  color: #fff;
  padding: 12px 20px;
  border: none;
  border-radius: 4px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
  cursor: pointer;
}

.game-button:hover {
  background-color: #5a6268;
}

@media screen and (min-width: 768px) {
  .game-form {
    width: 50%;
  }
}
.red {
  color: red;
}
.green {
  color: green;
}
</style>
