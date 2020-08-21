---
layout: post
title:  Node Password Generator
categories: [NodeJS, JavaScript, CLI]
excerpt: We're often told to never use the same password. While that's true, it's often one of the most annoying aspects of registering for a new website. So below, I offer a short tutorial on creating an executable nodejs password generator. 
---

We're often told to never use the same password. While that's true, it's often one of the most annoying aspects of registering for a new website. So below, I offer a short tutorial on creating an executable nodejs password generator. 

## Making our file executable

Our first step is to actually create a file. Call it `pass.js`, if you'd like. 

Assuming you're working in a unix-like environment, the easiest way to make our file executable is by placing `#!/usr/bin/env node` at the top of your file. Then, in the terminal, run `chmod +x <filename>`. To execute the file, you'd simply type `./<filename>`, in our case, it's going to be `./pass.js`. Of course, you could also run it just using `node pass.js`.


## Let's get input

We want to tell the program how strong our password should be, so our first step is to write a function that gets some input. We're going to call this function `prompt()`, which will accept two parameters: message, and a callback function.


```js
#!/usr/bin/env node
'use strict';

const prompt = (message, callback) => {
  const stdin = process.stdin;
  const stdout = process.stdout;

  stdin.resume();
  stdout.write(message);

  stdin.once('data', (data) => {
    callback(data);
  });
};
```

**Note:** If you'd like to learn more about the nodejs `process`, check the node documentation: [NodeJS process](https://nodejs.org/api/process.html). For now, we're not going to worry about this, as it's too much to get into for the sake of this article. Just know that the above function is what's going to display our initial message when the program runs, as well as generate our password via the callback.

## Using prompt()

So now we have our prompt() function defined. We need to actually use it. Remember, prompt() is going to take two parameters. The first, `message`, is going to be a string, which is going to ask us how strong (based on length) we'd like the password to be. `callback` is going to be our callback function where we take the input and generate a password.

```js
prompt('PASSWORD STRENGTH: ', strength => {
  console.log('\n');

  const chars =
    'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz01234567899876543210!@#$^.';
  let password = '';

  for (let i = 0; i < strength; i++) {
    password += chars.charAt(Math.floor(Math.random() * chars.length));
  }

  console.log('__________________________\n');
  console.log(`Password: ${password}`);
  console.log('__________________________\n');

  process.exit();
});
```
The first `console.log()` we see isn't necessary, but it makes our program look a little better by logging a new line. So, we have a variable `chars` which holds all of the characters we're going to loop through in our for loop. `password` is going to be the password, of course, it's value being set in the body of our `for` loop. The `for` loop itself runs as many times as we want it to, based on `strength`. 

And there we have it, a basic NodeJS password generator.

**Note:** A second part of this article will become available sometime in the future, where we also get a random username. It will expand on the above code, introducing some other NodeJS features, such as the file system module, [fs](https://nodejs.org/api/fs.html).

## Full Code

```js
#!/usr/bin/env node
'use strict';

const prompt = (message, callback) => {
  const stdin = process.stdin;
  const stdout = process.stdout;

  stdin.resume();
  stdout.write(message);

  stdin.once('data', (data) => {
    callback(data);
  });
};

prompt('PASSWORD STRENGTH: ', (strength) => {
  console.log('\n');

  const chars =
    'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz01234567899876543210!@#$^.';
  let password = '';

  for (let i = 0; i < strength; i++) {
    password += chars.charAt(Math.floor(Math.random() * chars.length));
  }

  console.log('__________________________\n');
  console.log(`Password: ${password}`);
  console.log('__________________________\n');

  process.exit();
});
```