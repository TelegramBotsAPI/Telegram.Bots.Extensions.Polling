# Telegram.Bots.Extensions.Polling
> Integrate Long Polling with Telegram.Bots

### Getting Started

#### Configuring Long Polling with Telegram.Bots
```c#
using Microsoft.Extensions.DependencyInjection;
using Telegram.Bots;

...

IServiceCollection services = ...

services.AddBotClient("<bot-token>");
services.AddPolling<UpdateHandler>();
```

#### Configuring an Update Handler

```c#
using Telegram.Bots.Extensions.Polling;
using Telegram.Bots.Requests;
using Telegram.Bots.Types;

...

public class UpdateHandler : IUpdateHandler
{
  public Task HandleAsync(IBotClient bot, Update update, CancellationToken token)
  {
    return update switch
    {
      MessageUpdate u when u.Data is TextMessage message => EchoText(message),
      EditedMessageUpdate u when u.Data is TextMessage message => EchoTextAsReply(message),
      _ => Task.CompletedTask
    };

    Task EchoText(TextMessage message) =>
      bot.HandleAsync(new SendText(message.Chat.Id, message.Text), token);

    Task EchoTextAsReply(TextMessage message) =>
      bot.HandleAsync(new SendText(message.Chat.Id, message.Text)
      {
        ReplyToMessageId = message.Id
      }, token);
  }
}
```

### License

Telegram.Bots.Extensions.Polling is an extension for Telegram.Bots.  
Copyright © 2020  Aman Agnihotri (amanagnihotri@pm.me)

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Lesser General Public License as published
by the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Lesser General Public License for more details.

You should have received a copy of the GNU Lesser General Public License
along with This program.  If not, see [GNU Licenses](https://www.gnu.org/licenses/).

---
