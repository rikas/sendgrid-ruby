[![Travis Badge](https://travis-ci.org/sendgrid/sendgrid-ruby.svg?branch=master)](https://travis-ci.org/sendgrid/sendgrid-ruby)


**This library allows you to quickly and easily use the SendGrid Web API v3 via Ruby.**

Version 3.X.X+ of this library provides full support for all SendGrid [Web API v3](https://sendgrid.com/docs/API_Reference/Web_API_v3/index.html) endpoints, including the new [v3 /mail/send](https://sendgrid.com/blog/introducing-v3mailsend-sendgrids-new-mail-endpoint).

This library represents the beginning of a new path for SendGrid. We want this library to be community driven and SendGrid led. We need your help to realize this goal. To help make sure we are building the right things in the right order, we ask that you create [issues](https://github.com/sendgrid/sendgrid-ruby/issues) and [pull requests](https://github.com/sendgrid/sendgrid-ruby/blob/master/CONTRIBUTING.md) or simply upvote or comment on existing issues or pull requests.

Please browse the rest of this README for further detail.

We appreciate your continued support, thank you!

# Table of Contents

* [Installation](#installation)
* [Quick Start](#quick_start)
* [Usage](#usage)
* [Use Cases](#use_cases)
* [Announcements](#announcements)
* [Roadmap](#roadmap)
* [How to Contribute](#contribute)
* [Troubleshooting](#troubleshooting)
* [About](#about)

<a name="installation"></a>
# Installation

## Prerequisites

- Ruby version 2.2
- The SendGrid service, starting at the [free level](https://sendgrid.com/free?source=sendgrid-ruby)

## Setup Environment Variables

Update the development environment with your [SENDGRID_API_KEY](https://app.sendgrid.com/settings/api_keys), for example:

```bash
echo "export SENDGRID_API_KEY='YOUR_API_KEY'" > sendgrid.env
echo "sendgrid.env" >> .gitignore
source ./sendgrid.env
```
## Install Package

Add this line to your application's Gemfile:

```bash
gem 'sendgrid-ruby'
```

And then execute:

```bash
bundle
```

Or install it yourself using:

```bash
gem install sendgrid-ruby
```

## Dependencies

- [Ruby-HTTP-Client](https://github.com/sendgrid/ruby-http-client)

<a name="quick_start"></a>
# Quick Start

## Hello Email

The following is the minimum needed code to send an email with the [/mail/send Helper](https://github.com/sendgrid/sendgrid-ruby/tree/master/lib/sendgrid/helpers/mail) ([here](https://github.com/sendgrid/sendgrid-ruby/blob/master/examples/helpers/mail/example.rb#L21) is a full example):

### With Mail Helper Class

```ruby
require 'sendgrid-ruby'
include SendGrid

from = Email.new(email: 'test@example.com')
subject = 'Hello World from the SendGrid Ruby Library!'
to = Email.new(email: 'test@example.com')
content = Content.new(type: 'text/plain', value: 'Hello, Email!')
mail = Mail.new(from, subject, to, content)

sg = SendGrid::API.new(api_key: ENV['SENDGRID_API_KEY'])
response = sg.client.mail._('send').post(request_body: mail.to_json)
puts response.status_code
puts response.body
puts response.headers
```

For more complex scenarios, please do not use the above constructor and instead build your own personalization object as [demonstrated here](https://github.com/sendgrid/sendgrid-ruby/blob/master/examples/helpers/mail/example.rb#L21).

### Without Mail Helper Class

The following is the minimum needed code to send an email without the /mail/send Helper ([here](https://github.com/sendgrid/sendgrid-ruby/blob/master/examples/mail/mail.rb#L26) is a full example):

```ruby
require 'sendgrid-ruby'
include SendGrid

data = JSON.parse('{
  "personalizations": [
    {
      "to": [
        {
          "email": "test@example.com"
        }
      ],
      "subject": "Hello World from the SendGrid Ruby Library!"
    }
  ],
  "from": {
    "email": "test@example.com"
  },
  "content": [
    {
      "type": "text/plain",
      "value": "Hello, Email!"
    }
  ]
}')
sg = SendGrid::API.new(api_key: ENV['SENDGRID_API_KEY'])
response = sg.client.mail._("send").post(request_body: data)
puts response.status_code
puts response.body
puts response.headers
```

## General v3 Web API Usage (With Fluent Interface)

```ruby
require 'sendgrid-ruby'
sg = SendGrid::API.new(api_key: ENV['SENDGRID_API_KEY'])
response = sg.client.suppression.bounces.get()
puts response.status_code
puts response.body
puts response.headers
```

## General v3 Web API Usage (Without Fluent Interface)

```ruby
require 'sendgrid-ruby'
sg = SendGrid::API.new(api_key: ENV['SENDGRID_API_KEY'])
response = sg.client._("suppression/bounces").get()
puts response.status_code
puts response.body
puts response.headers
```

<a name="usage"></a>
# Usage

- [SendGrid Docs](https://sendgrid.com/docs/API_Reference/index.html)
- [Library Usage Docs](https://github.com/sendgrid/sendgrid-ruby/tree/master/USAGE.md)
- [Example Code](https://github.com/sendgrid/sendgrid-ruby/tree/master/examples)
- [How-to: Migration from v2 to v3](https://sendgrid.com/docs/Classroom/Send/v3_Mail_Send/how_to_migrate_from_v2_to_v3_mail_send.html)
- [v3 Web API Mail Send Helper](https://github.com/sendgrid/sendgrid-python/tree/master/sendgrid/helpers/mail) - build a request object payload for a v3 /mail/send API call.

<a name="use_cases">
# Use Cases

[Examples of common API use cases](https://github.com/sendgrid/sendgrid-ruby/blob/master/USE_CASES.md), such as how to send an email with a transactional template.

<a name="announcements"></a>
# Announcements

All updates to this library is documented in our [CHANGELOG](https://github.com/sendgrid/sendgrid-ruby/blob/master/CHANGELOG.md) and [releases](https://github.com/sendgrid/sendgrid-ruby/releases).

<a name="roadmap"></a>
# Roadmap

If you are interested in the future direction of this project, please take a look at our open [issues](https://github.com/sendgrid/sendgrid-ruby/issues) and [pull requests](https://github.com/sendgrid/sendgrid-ruby/pulls). We would love to hear your feedback.

<a name="contribute"></a>
# How to Contribute

We encourage contribution to our libraries (you might even score some nifty swag), please see our [CONTRIBUTING](https://github.com/sendgrid/sendgrid-ruby/tree/master/CONTRIBUTING.md) guide for details.

- [Feature Request](https://github.com/sendgrid/sendgrid-ruby/tree/master/CONTRIBUTING.md#feature_request)
- [Bug Reports](https://github.com/sendgrid/sendgrid-ruby/tree/master/CONTRIBUTING.md#submit_a_bug_report)
- [Sign the CLA to Create a Pull Request](https://github.com/sendgrid/sendgrid-ruby/tree/master/CONTRIBUTING.md#cla)
- [Improvements to the Codebase](https://github.com/sendgrid/sendgrid-ruby/tree/master/CONTRIBUTING.md#improvements_to_the_codebase)

<a name="troubleshooting"></a>
# Troubleshooting

Please see our [troubleshooting guide](https://github.com/sendgrid/sendgrid-ruby/blob/master/TROUBLESHOOTING.md) for common library issues.


<a name="about"></a>
# About

sendgrid-ruby is guided and supported by the SendGrid [Developer Experience Team](mailto:dx@sendgrid.com).

sendgrid-ruby is maintained and funded by SendGrid, Inc. The names and logos for sendgrid-ruby are trademarks of SendGrid, Inc.

![SendGrid Logo]
(https://uiux.s3.amazonaws.com/2016-logos/email-logo%402x.png)
