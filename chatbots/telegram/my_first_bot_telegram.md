# My first chatbot Telegram 

### To initialise new nodejs project:
- npm init
- npm init -y (for default settings)

### To install packages/libraries on npm

- npm install `<package name>`
- npm i `<package name>`
- npm install -g `<package name>` (global)


### Initialise new project nodejs
``` shell
npm init 
```
...insert image...

### Initialise new project nodejs (default settings)
```shell
npm init -y 
```
...insert image ...


### Install telegraf package

``` shell
npm i telegraf
```

### The first bot
``` javascript 
const Telegraf  = require ('telegraf');

const bot  = new Telegraf('TOKEN');

//code
// /start
bot.start((ctx) => {
    ctx.reply(ctx.from.first_name + " hai digitato il comando: " + ctx.message.text);
    console.log(ctx.from);
    console.log(ctx.chat);
    console.log(ctx.from.first_name + " hai digitato il comando: " + ctx.message.text);
    console.log(ctx.updateSubTypes);
})

bot.hears(["ciao", "Ciao"], (ctx) =>{
    ctx.reply("Ciao a te" + ctx.from.first_name );
});


bot.on("text",(ctx) =>{
    let msg = ctx.message.text;
    ctx.reply("Ciao " + ctx.from.first_name + " hai digitato: " + ctx.message.text);
    console.log(msg);
});


bot.launch();
```
 

 ### Bot telegram
 ``` javascript
    const Telegraf = require('telgraf');

    const bot = new Telegraf('Token');

    //Start the bot
    bot.lunch()

 ``` 

 ### Start and help commands

 ```javascritp

 ```

