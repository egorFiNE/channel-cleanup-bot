/* eslint-disable camelcase */
import TelegramBot from 'node-telegram-bot-api';

process.loadEnvFile();

const bot = new TelegramBot(process.env.TELEGRAM_TOKEN, { polling: true });

const NOT_WELCOME_MESSAGE = [
  "Hi. I'm a private bot managing a count of specific Telegram channel.",
  "There is nothing I can do for you, so goodbye and have a nice day :-)\n\n",
  "Привет! Я частный бот, работающий только на парочке секретных телеграм каналов,",
  "поэтому ничем не могу вам быть полезен. До свидания и хорошего дня! :-)\n\n"
].join(" ");

function processPrivateMessage(msg) {
  const text = (msg.text || '').trim();
  const chatId = String(msg.chat.id);

  if (text == '/ping') {
    bot.sendMessage(chatId, "Pong!");
    return;
  }

  bot.sendMessage(chatId, NOT_WELCOME_MESSAGE, { parse_mode: 'Markdown' });
}

/**********************************/

bot.on('message', msg => {
  const isPrivate = msg.chat?.type != 'supergroup';

  if (!isPrivate) {
    return;
  }

  processPrivateMessage(msg);
});

bot.on('new_chat_members', async msg => {
  for (const member of msg.new_chat_members) {
    if (member.is_bot) {
      continue;
    }

    console.log("Deleted new_chat_members message", msg);
    bot.deleteMessage(msg.chat.id, msg.message_id);
  }
});

bot.on('polling_error', error => {
  console.log('polling_error');
  console.log(error);
});
