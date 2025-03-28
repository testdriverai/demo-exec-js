# Execute JS In TestDriver.ai

Let's say you want to use some custom JS within your test to hit an API. 

As of `5.1.0` you can execute any NodeJS script complete with NPM support using the `exec` command! 

This repo is a demo of running custom npm packages in TestDriver. 

## Running this demo

Clone this repo, then run:

```
npm install
testdriverai run testdriver/testdriver.yaml
```

![CleanShot 2025-03-27 at 20 52 49@2x](https://github.com/user-attachments/assets/d6f5cbb4-b1b3-49b1-878c-dfcbfc3ce96f)


## How it works

### Params
- `js` - The NodeJS script to run. Output must be defined as `result`
- `output` - The variable name available as `${OUTPUT.var}` in future steps


```yaml
version: 5.1.0
session: 67e57d614dc25283aa0872a9
steps:
  - prompt: Log in with the totp
    commands:
      - command: exec
        output: totp 
        js: |
          const { TOTP } = require("totp-generator");
          let otp = TOTP.generate("JBSWY3DPEB3W64TMMQQQ").otp;
          result = otp;
      - command: type
        text: ${OUTPUT.totp}
```

### Protips

- Don't overwrite `result`
- Make sure to `npm install` locally (and within `prerun` when using the GitHub action)
- NodeJS code executes in the same context as the calling process, on the host machine. It uses [VM](https://nodejs.org/api/vm.html) internally.

### Gotchas

- This can only be deployed on hosted Windows runners, hosted Linux runners do not support `prerun` yet

## Using NPM Modules From Scratch

```
npm init
npm install totp-generator --save
```


