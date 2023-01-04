<template>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="8">
        <v-card class="logo py-4 d-flex align-center flex-column">
          <div class="text-h5 text-center">Welcome to the OpeanAI Chatbot</div>
          <div class="text-caption text-center">Please ask your question below.</div>
          <v-container fluid>
            <v-textarea
              v-model="textArea"
              clearable
              rows="1"
              no-resize
              clear-icon="mdi-close-circle"
              :label="label"
            ></v-textarea>
            <v-btn
              :disabled="(textArea == '' || textArea == null)"
              class="float-right"
              @click="sumbmitted"
            >
              Submit
            </v-btn>
          </v-container>
  
          <div class="d-flex justify-center">
            <v-progress-circular v-if="loading" indeterminate color="deep-orange lighten-2"></v-progress-circular>
          </div>
          
          <v-container fluid>
            <v-list rounded >
              <v-subheader v-if="runningQandAs.length > 0" class="text-center text-h5">
                  Chat History 
                  <v-btn icon x-small @click="fullReset">
                    <v-icon>mdi-delete</v-icon>
                </v-btn>
            </v-subheader>
            <v-expand-transition>
              <v-list-item-group  v-if="runningQandAs.length > 0" ref="runningQandAs" color="primary" class="overflow-auto runningQandAs">
                <v-list-item  v-for="(QandA, i) in runningQandAs" :key="i">
                  <v-list-item-icon>
                    <v-icon>mdi-chat-question</v-icon>
                  </v-list-item-icon>
                  <v-list-item-content>
                    <v-list-item-title>{{QandA.question}}</v-list-item-title>
                    <v-list-item-subtitle>
                      {{QandA.answer}}
                    </v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>
              </v-list-item-group>
            </v-expand-transition>
            </v-list>
          </v-container>
        </v-card>
      </v-col>
    </v-row>
  </template>
  
  <script>
  
  
  export default {
    name: 'IndexPage',
    data() {
      return {
        loading: false,
        textArea: "What is a good used car to buy?",
        label: "Please write your question.",
        prompts: [],
        runningQandAs: [],
        permissableEndCharacters: ["?", ".", "!"]
      }
    },
    methods: {
      async sumbmitted(){
        this.loading = true
        let question = this.textArea
        if(!this.permissableEndCharacters.includes(question.charAt(question.length-1))){
          question = question + "."
        }
        this.prompts.push(`Q. ${question}`)
        const { Configuration, OpenAIApi } = require("openai");
        const configuration = new Configuration({
          apiKey: this.$config.openAIApiKey,
        });
        const openai = new OpenAIApi(configuration);
        const response = await openai.createCompletion({
          model: "text-davinci-003", // USE this model as defined in the Q-A playground?
          prompt: this.prompts.join('\n'),
          max_tokens: 1000,
          temperature: 0,
        });
        const responseText = response.data.choices[0].text
        this.prompts.push(`${responseText}`)
        this.runningQandAs.push({
          question,
          answer: responseText
        })
        this.textArea = ''
        this.loading = false
        this.$nextTick(() => {
          this.scrollToEnd()
        });
      },
      scrollToEnd(){
        this.$refs.runningQandAs.$el.scrollTop = this.$refs.runningQandAs.$el.scrollHeight + 100
      },
      fullReset(){
        this.prompts, this.runningQandAs = []
      }
    },
  }
  </script>
  
  <style>
  .v-list-item__title, .v-list-item__subtitle {
    overflow: auto !important;
    text-overflow: ellipsis !important;
    white-space: initial !important;
  }

  .runningQandAs{
    height: 250px;
    border: 1px solid #dedede;
    border-radius: 5px;
    padding: 5px;
    /* box-shadow: 0px 2px 1px -2px rgb(255 255 255 / 20%), 0px 2px 2px 0px rgb(0 0 0 / 14%), 0px 1px 5px 0px rgb(255 255 255 / 12%); */
  }
  </style>
  