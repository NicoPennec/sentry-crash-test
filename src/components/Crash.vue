<template>
  <div class="crash">
    <h1>{{ title }}</h1>
    <h3>Pease click on the blue buttons to crash this awesome app.</h3>
    <section class="panel panel-default">
      <h2 class="panel-heading">Error Generator</h2>
      <p>
        <label>Click to generate a</label><br>
        <button type="button" class="btn btn-primary" @click="generateError">Random Crash 💥</button>
      </p>
      <p v-if="error" class="alert" v-bind:class="'alert-' + error.type">{{ error.msg }}</p>
    </section>
    <section class="panel panel-default">
      <h2 class="panel-heading">Dummy Form</h2>
      <form v-on:submit.prevent="onSubmit" v-if="!submit">
        <p>
          <label>Your nickname <input id="nickname" :value="nickname" type="text" value="@twitter" class="form-control"></label>
        </p>
        <p>
          <label>Your secret code <input id="secret" :value="secret" type="password" class="form-control"></label>
        </p>
        <p>
          <label>Your message <textarea id="message" :value="message" placeholder="Enter an awesome message" class="form-control"></textarea></label>
        </p>
        <p>
          <input type="submit" class="btn btn-primary" value="Submit">
        </p>
      </form>
      <form v-on:submit.prevent="onSubmit" v-else>
        <img src="../assets/break.jpg" alt="break">
        <h3>Sorry, your request is breaking bad :-(</h3>
        <p>
          <input type="submit" class="btn btn-primary" value="Retry ?">
        </p>
      </form>
    </section>
    <section class="panel panel-default">
      <h2 class="panel-heading">User Feedback</h2>
      <p>
        <button type="button" @click="onFeedback" class="btn btn-primary">See the report dialog in action</button>
      </p>
    </section>
    <hr>
    <h2>Essential Links</h2>
    <ul>
      <li><a href="https://sentry.io" target="_blank">Sentry.io</a></li>
      <li><a href="https://docs.sentry.io/" target="_blank">Sentry Docs</a></li>
      <li><a href="http://pennec.io" target="_blank">Pennec.io</a></li>
      <br><br>
      <li><a href="http://www.breizhcamp.org/" target="_blank">BreizhCamp.org</a></li>
    </ul>
  </div>
</template>

<script>
import Raven from 'raven-js'

export default {
  name: 'crash',
  data () {
    return {
      title: 'Welcome the Sentry Crash Test',
      nickname: 'BreizhCamp',
      secret: null,
      message: null,
      submit: false,
      error: null
    }
  },
  methods: {
    generateError: function () {
      this.error = {
        code: Math.floor(Math.random() * 8),
        msg: null,
        type: 'danger'
      }
      console.log('error code', this.error.code)

      switch (this.error.code) {
        case 1:
          this.error.msg = 'divide by zero'
          this.data = 1 / 0
          break
        case 2:
          this.error.msg = 'wrong function'
          this.data = Math.random().floor()
          break
        case 3:
          this.error.msg = 'missing #unknow element in DOM'
          document.getElementById('unknow').value
          break
        case 4:
          let code = 500 + Math.floor(Math.random() * 10)
          let msg = `server error [code error = ${code}]`
          this.error.msg = msg
          setTimeout(() => { throw new Error(msg) }, 0)
          break
        case 5:
          this.error.msg = 'fetch() failed'
          this.data = fetch('yolo').then(function (response) {
            return response.data()
          })
          break
        case 6:
          this.error.type = 'warning'
          this.error.msg = 'Warning !!! it should never have happen'
          console.warn(this.error.msg)
          Raven.captureMessage(this.error.msg, { level: 'warning' })
          break
        case 7:
          this.error.type = 'info'
          this.error.msg = 'Lost connection'
          console.warn(this.error.msg)
          Raven.captureMessage(this.error.msg, { level: 'info' })
          break
        default:
          this.error.msg = 'Oops! try again...'
          throw new Error(this.error.msg)
      }
    },
    onSubmit: function () {
      this.submit = !this.submit
      throw new Error('Oops! the form failed...')
    },
    onFeedback: function () {
      console.log('feedback')
      this.error = {
        msg: ''
      }

      try {
        throw new Error('Oops! Please, try again...')
      } catch (err) {
        Raven.captureException(err)
        Raven.showReportDialog()
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

h1 {
  margin-bottom: 40px;
}

h2 {
  margin-top: 0;
  margin-bottom: 40px;
}

h3 {

}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

label {
  text-align: left;
}

hr {
  margin-top: 40px;
  margin-bottom: 20px;
}

.panel {
  margin-top: 40px;
}

.alert {
  margin: 20px auto;
  max-width: 500px;
  font-weight: bold;
}
</style>
