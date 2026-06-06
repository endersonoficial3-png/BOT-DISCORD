# BOT-DISCORD
BOT ANTI RAID
const { Client, GatewayIntentBits } = require("discord.js");

const client = new Client({
  intents: [
    GatewayIntentBits.Guilds,
    GatewayIntentBits.GuildMembers
  ]
});

const joins = new Map();

// Anti-raid básico
client.on("guildMemberAdd", member => {
  const now = Date.now();
  const id = member.guild.id;

  if (!joins.has(id)) joins.set(id, []);

  let data = joins.get(id);

  data.push(now);

  data = data.filter(t => now - t < 10000);

  joins.set(id, data);

  if (data.length >= 5) {
    console.log("⚠️ Posible raid detectado en:", member.guild.name);
  }
});

client.once("ready", () => {
  console.log(`Bot encendido como ${client.user.tag}`);
});

client.login(process.env.TOKEN);
