# acv-openai
> A simple to use chatbot powered by by OpenAI.

## Getting Started
This application relies on having a valid and working OpenAI API key. To retrieve your key, visit the [OpenAI API key generation page](https://beta.openai.com/account/api-keys). Note: key generation requires that you register an account on OpenAI free of charge.

Once an API key has been generated, store its value into the `OPEN_AI_API_KEY` variable located in this repositorys `.env` file.

Afterwords, perform the steps below to get the project up and running.

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

## Usage
Once running, navigation to you [localhost:3000](http://localhost:3000). To interact with the chatbot, simply write a prompt inside of the text box, and press submit. Once the chatbot has a response, a chat history will appear displaying an previously submitted prompts and responses. To clear the conversation and start from scratch, simply click the trash icon located on the right of _Chat History_.

### OpenAI API Documentation
For futher information on integrating with OpenAI, checkout out their [documentation.](https://beta.openai.com/docs/introduction)
