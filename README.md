<p align="center">
  <a href="http://nestjs.com/" target="blank">
    <img src="https://nestjs.com/img/logo-small.svg" width="120" alt="Nest Logo" />
  </a>
</p>

<p align="center">
  📨 Build and send emails in Nest framework using React.js
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@webtre/nestjs-mailer-react-adapter"><img src="https://img.shields.io/npm/v/@webtre/nestjs-mailer-react-adapter.svg" alt="NPM Version" /></a>
  <a href="https://www.npmjs.com/package/@webtre/nestjs-mailer-react-adapter"><img src="https://img.shields.io/npm/l/@webtre/nestjs-mailer-react-adapter.svg" alt="Package License" /></a>
  <a href="https://www.npmjs.com/package/@webtre/nestjs-mailer-react-adapter"><img src="https://img.shields.io/npm/dm/@webtre/nestjs-mailer-react-adapter.svg" alt="NPM Downloads" /></a>
</p>

## Features

- ⚡️ Write your mail templates in [React](https://github.com/facebook/react/) and [TypeScript](https://www.typescriptlang.org/)

- 🦾 Write testable mail templates intended for mail clients.

- ⛔ No more template not found or sending blank emails.

- ⛔ No more issues of missing context / variables from template.

- 💌 Built on [`react-email`](https://github.com/resendlabs/react-email) - it reduces the pain of coding responsive emails with dark mode support.

## Installation

> This library is an adapter for the [`@nestjs-modules/mailer`](https://github.com/nest-modules/mailer) module. If you're yet to have it installed, do so by running the command below.

```sh
npm install @nestjs-modules/mailer nodemailer
```

Install this library

```sh
npm install @webtre/nestjs-mailer-react-adapter
```

### Getting Started

To add support for react, ensure this is present under compiler options in your `tsconfig.json`

```json
  "jsx": "react-jsx"
```

1. Configuration

```javascript
// src/app.module.ts
import { Module } from "@nestjs/common";
import { MailerModule } from "@nestjs-modules/mailer";
import { ReactAdapter } from "@webtre/nestjs-mailer-react-adapter";

@Module({
  imports: [
    MailerModule.forRoot({
      transport: {
        host: "smtp.domain.com",
        port: 2525,
        secure: false,
        auth: {
          user: "user@domain.com",
          pass: "password",
        },
      },
      defaults: {
        from: '"Webtre Technologies" <hello@domain.com>',
      },
      template: {
        dir: __dirname + "/templates",
        // Use the adapter
        adapter: new ReactAdapter(),

        // Or with optional config
        adapter: new ReactAdapter({
          pretty: false,
          plainText: true,
        }),
      },
    }),
  ],
})
export class AppModule {}
```

2. Service Provider

```javascript
import { Injectable } from '@nestjs/common';
import { MailerService } from '@nestjs-modules/mailer';

@Injectable()
export class ExampleService {
  constructor(private readonly mailerService: MailerService) {}

  async public example(): void {
    await this.mailerService
      .sendMail({
        to: 'john@domain.com',
        subject: 'Testing react template',
        template: 'welcome', // The compiled extension is appended automatically.
        context: {
          // Data to be passed as props to your template.
          code: '123456',
          name: 'John Doe',
        },
      });
  }
}
```

3. React Template (ensure its a `default` export)

```javascript
// src/templates/welcome.tsx
interface WelcomeProps {
  code: string;
  name: string;
}

export default function Welcome({ name, code }: WelcomeProps) {
  return (
    <div>
      Hi {name}, thanks for signing up. Your code is {code}
    </div>
  );
}
```

## Credits

- [`react-email`](https://github.com/resendlabs/react-email) - build and send emails using React
- [`@nestjs-modules/mailer`](https://github.com/nest-modules/mailer) - a mailer module for Nest framework (node.js)

## License

[MIT](./LICENSE) License © 2022 [Webtre Technologies](https://github.com/webtretech)
