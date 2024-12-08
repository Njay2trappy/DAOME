const { Telegraf, Markup } = require("telegraf");
const Web3 = require("web3");
const fs = require("fs");

// Bot token and RPC URL
const BOT_TOKEN = "Your_bot_token";
const RPC_URL = "https://rpc.airdao.io/";

// Initialize Web3
const web3 = new Web3(new Web3.providers.HttpProvider(RPC_URL));
console.log(`✅ Web3 initialized with RPC URL: ${RPC_URL}`);

// ERC-20 ABI
const ERC20_ABI = [
	{
		"inputs": [
			{
				"internalType": "string",
				"name": "name",
				"type": "string"
			},
			{
				"internalType": "string",
				"name": "symbol",
				"type": "string"
			},
			{
				"internalType": "uint256",
				"name": "initialSupply",
				"type": "uint256"
			}
		],
		"stateMutability": "nonpayable",
		"type": "constructor"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "spender",
				"type": "address"
			},
			{
				"internalType": "uint256",
				"name": "allowance",
				"type": "uint256"
			},
			{
				"internalType": "uint256",
				"name": "needed",
				"type": "uint256"
			}
		],
		"name": "ERC20InsufficientAllowance",
		"type": "error"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "sender",
				"type": "address"
			},
			{
				"internalType": "uint256",
				"name": "balance",
				"type": "uint256"
			},
			{
				"internalType": "uint256",
				"name": "needed",
				"type": "uint256"
			}
		],
		"name": "ERC20InsufficientBalance",
		"type": "error"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "approver",
				"type": "address"
			}
		],
		"name": "ERC20InvalidApprover",
		"type": "error"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "receiver",
				"type": "address"
			}
		],
		"name": "ERC20InvalidReceiver",
		"type": "error"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "sender",
				"type": "address"
			}
		],
		"name": "ERC20InvalidSender",
		"type": "error"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "spender",
				"type": "address"
			}
		],
		"name": "ERC20InvalidSpender",
		"type": "error"
	},
	{
		"anonymous": false,
		"inputs": [
			{
				"indexed": true,
				"internalType": "address",
				"name": "owner",
				"type": "address"
			},
			{
				"indexed": true,
				"internalType": "address",
				"name": "spender",
				"type": "address"
			},
			{
				"indexed": false,
				"internalType": "uint256",
				"name": "value",
				"type": "uint256"
			}
		],
		"name": "Approval",
		"type": "event"
	},
	{
		"anonymous": false,
		"inputs": [
			{
				"indexed": true,
				"internalType": "address",
				"name": "from",
				"type": "address"
			},
			{
				"indexed": true,
				"internalType": "address",
				"name": "to",
				"type": "address"
			},
			{
				"indexed": false,
				"internalType": "uint256",
				"name": "value",
				"type": "uint256"
			}
		],
		"name": "Transfer",
		"type": "event"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "owner",
				"type": "address"
			},
			{
				"internalType": "address",
				"name": "spender",
				"type": "address"
			}
		],
		"name": "allowance",
		"outputs": [
			{
				"internalType": "uint256",
				"name": "",
				"type": "uint256"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "spender",
				"type": "address"
			},
			{
				"internalType": "uint256",
				"name": "value",
				"type": "uint256"
			}
		],
		"name": "approve",
		"outputs": [
			{
				"internalType": "bool",
				"name": "",
				"type": "bool"
			}
		],
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "account",
				"type": "address"
			}
		],
		"name": "balanceOf",
		"outputs": [
			{
				"internalType": "uint256",
				"name": "",
				"type": "uint256"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "decimals",
		"outputs": [
			{
				"internalType": "uint8",
				"name": "",
				"type": "uint8"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "name",
		"outputs": [
			{
				"internalType": "string",
				"name": "",
				"type": "string"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "symbol",
		"outputs": [
			{
				"internalType": "string",
				"name": "",
				"type": "string"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [],
		"name": "totalSupply",
		"outputs": [
			{
				"internalType": "uint256",
				"name": "",
				"type": "uint256"
			}
		],
		"stateMutability": "view",
		"type": "function"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "to",
				"type": "address"
			},
			{
				"internalType": "uint256",
				"name": "value",
				"type": "uint256"
			}
		],
		"name": "transfer",
		"outputs": [
			{
				"internalType": "bool",
				"name": "",
				"type": "bool"
			}
		],
		"stateMutability": "nonpayable",
		"type": "function"
	},
	{
		"inputs": [
			{
				"internalType": "address",
				"name": "from",
				"type": "address"
			},
			{
				"internalType": "address",
				"name": "to",
				"type": "address"
			},
			{
				"internalType": "uint256",
				"name": "value",
				"type": "uint256"
			}
		],
		"name": "transferFrom",
		"outputs": [
			{
				"internalType": "bool",
				"name": "",
				"type": "bool"
			}
		],
		"stateMutability": "nonpayable",
		"type": "function"
	}
];

// File paths
const ORDER_BOOK_FILE = "order_book.json";
const NEW_WALLET_FILE = "New_wallet.json";

// Session timeout (in milliseconds)
const SESSION_TIMEOUT = 10 * 60 * 1000; // 10 minutes

// Load JSON data
const loadJSON = (filePath) => {
  try {
    if (fs.existsSync(filePath)) return JSON.parse(fs.readFileSync(filePath, "utf8"));
    return [];
  } catch (error) {
    console.error(`Error loading ${filePath}:`, error);
    return [];
  }
};

const saveJSON = (filePath, data) => {
  try {
    fs.writeFileSync(filePath, JSON.stringify(data, null, 4));
    console.log(`✅ Data saved to ${filePath}`);
  } catch (error) {
    console.error(`❌ Error saving ${filePath}:`, error);
  }
};

const orderBook = loadJSON(ORDER_BOOK_FILE);
const wallets = loadJSON(NEW_WALLET_FILE);

// Centralized function to retrieve the generated wallet
const getGeneratedWallet = (publicAddress) => {
  return wallets.find((w) => w.publicAddress === publicAddress);
};

// Retry RPC calls
const retryRpcCall = async (func, retries = 3, delay = 1000) => {
  for (let attempt = 1; attempt <= retries; attempt++) {
    try {
      return await func();
    } catch (error) {
      console.error(`❌ RPC Call Failed (Attempt ${attempt}):`, error.message);
      if (attempt < retries) await new Promise((res) => setTimeout(res, delay));
    }
  }
  throw new Error("All RPC call attempts failed.");
};

// Initialize bot and sessions
const bot = new Telegraf(BOT_TOKEN);
const userSessions = {}; // User session data

// Start command
bot.start((ctx) => {
  ctx.reply("👋 Welcome to Daome Trading Bot! Use /trade to start trading tokens.");
});

// Trade command
bot.command("trade", (ctx) => {
  const chatId = ctx.chat.id;

  if (userSessions[chatId]) clearTimeout(userSessions[chatId].timeoutId);
  userSessions[chatId] = {
    timeoutId: startSessionTimeout(chatId),
    currentStep: "contractAddress",
  };

  ctx.reply("🔍 Enter the contract address of the token you want to trade.", Markup.inlineKeyboard([[Markup.button.callback("Cancel", "action_cancel")]]));
});

// Cancel trade session
bot.action("action_cancel", (ctx) => {
  const chatId = ctx.chat.id;

  if (userSessions[chatId]) {
    clearTimeout(userSessions[chatId].timeoutId);
    delete userSessions[chatId];
  }

  ctx.reply("❌ Trade session canceled. Use /trade to start over.");
});

// Handle user inputs
bot.on("text", async (ctx) => {
  const chatId = ctx.chat.id;
  const userSession = userSessions[chatId];

  if (!userSession) return ctx.reply("❌ Please start a trade using /trade.");

  resetSessionTimeout(chatId);

  const { currentStep } = userSession;

  switch (currentStep) {
    case "contractAddress":
      handleContractAddress(ctx, chatId);
      break;
    case "amount":
      handleAmountInput(ctx, chatId);
      break;
    case "privateKey":
      handlePrivateKeyInput(ctx, chatId);
      break;
    default:
      ctx.reply("❌ Invalid action. Please start over using /trade.");
  }
});

// Handle contract address input
const handleContractAddress = (ctx, chatId) => {
  const contractAddress = ctx.message.text.trim();
  const token = orderBook.find((t) => t.contractAddress === contractAddress);

  if (!token) {
    return ctx.reply("❌ Token not found in the order book. Please check the contract address.");
  }

  userSessions[chatId].tokenDetails = token;
  userSessions[chatId].currentStep = null;

  ctx.reply(
    `🎉 Token Found!\n🏷️ Name: ${token.name}\n💲 Symbol: ${token.symbol}\n📦 Price: ${token.price} AMB\n💰 Market Cap: ${token.marketCap} AMB\n\n💸 Choose an action:`,
    Markup.inlineKeyboard([
      [Markup.button.callback("Buy", "action_buy"), Markup.button.callback("Sell", "action_sell")],
      [Markup.button.callback("Cancel", "action_cancel")],
    ])
  );
};

// Buy button action
bot.action("action_buy", (ctx) => {
  const chatId = ctx.chat.id;
  const userSession = userSessions[chatId];

  if (!userSession || !userSession.tokenDetails) {
    return ctx.reply("❌ You need to select a token first. Use /trade to start.");
  }

  resetSessionTimeout(chatId);

  ctx.reply("💳 Enter the amount in AMB to buy:");
  userSession.currentStep = "amount";
  userSession.currentAction = "buy";
});

// Sell button action
bot.action("action_sell", (ctx) => {
  const chatId = ctx.chat.id;
  const userSession = userSessions[chatId];

  if (!userSession || !userSession.tokenDetails) {
    return ctx.reply("❌ You need to select a token first. Use /trade to start.");
  }

  resetSessionTimeout(chatId);

  ctx.reply("📦 Enter the quantity of tokens to sell:");
  userSession.currentStep = "amount";
  userSession.currentAction = "sell";
});

// Handle amount input
const handleAmountInput = (ctx, chatId) => {
  const userSession = userSessions[chatId];
  const amount = parseFloat(ctx.message.text);

  if (isNaN(amount) || amount <= 0) {
    return ctx.reply("❌ Please enter a valid amount.");
  }

  const { tokenDetails, currentAction } = userSession;

  if (currentAction === "buy") {
    const quantity = Math.floor(amount / tokenDetails.price);
    userSession.currentAmount = amount;
    userSession.currentQuantity = quantity;

    ctx.reply(`📊 You will receive ${quantity} ${tokenDetails.symbol}. Send your private key to sign the transaction.`);
  } else if (currentAction === "sell") {
    const ambAmount = Math.floor(amount * tokenDetails.price);
    userSession.currentAmount = ambAmount;
    userSession.currentQuantity = amount;

    ctx.reply(`💸 You will receive ${ambAmount} AMB. Send your private key to sign the transaction.`);
  }

  userSession.currentStep = "privateKey";
};

// Handle private key input
const handlePrivateKeyInput = async (ctx, chatId) => {
  const userSession = userSessions[chatId];
  const userPrivateKey = ctx.message.text.trim();

  try {
    const { tokenDetails, currentAction, currentAmount, currentQuantity } = userSession;
    const payerAccount = web3.eth.accounts.privateKeyToAccount(userPrivateKey);
    const generatedWallet = getGeneratedWallet(tokenDetails.mintedTo);

    if (!generatedWallet) {
      ctx.reply(`❌ Generated wallet for ${tokenDetails.name} not found.`);
      console.error("❌ Generated wallet not found:", tokenDetails.mintedTo);
      return;
    }

    const gasPrice = await retryRpcCall(() => web3.eth.getGasPrice());

    if (currentAction === "buy") {
      const fundTx = {
        from: payerAccount.address,
        to: tokenDetails.mintedTo,
        value: web3.utils.toWei(currentAmount.toString(), "ether"),
        gas: 21000,
        gasPrice,
      };

      const signedFundTx = await web3.eth.accounts.signTransaction(fundTx, userPrivateKey);
      await web3.eth.sendSignedTransaction(signedFundTx.rawTransaction);

      const contract = new web3.eth.Contract(ERC20_ABI, tokenDetails.contractAddress);

      // Check generated wallet token balance
      const balance = await contract.methods.balanceOf(generatedWallet.publicAddress).call();
      if (parseInt(balance) < currentQuantity) {
        ctx.reply("❌ Insufficient token balance in generated wallet.");
        console.error("❌ Token balance insufficient:", balance);
        return;
      }

      await contract.methods
        .transfer(payerAccount.address, currentQuantity)
        .send({ from: generatedWallet.publicAddress, gas: 21000 });

      ctx.reply(`✅ Successfully bought ${currentQuantity} ${tokenDetails.symbol}!`);
    } else if (currentAction === "sell") {
      const contract = new web3.eth.Contract(ERC20_ABI, tokenDetails.contractAddress);

      // Check user token balance
      const balance = await contract.methods.balanceOf(payerAccount.address).call();
      if (parseInt(balance) < currentQuantity) {
        ctx.reply("❌ Insufficient token balance in user wallet.");
        console.error("❌ User token balance insufficient:", balance);
        return;
      }

      await contract.methods
        .transfer(tokenDetails.mintedTo, currentQuantity)
        .send({ from: payerAccount.address, gas: 100000 });

      const fundTx = {
        from: generatedWallet.publicAddress,
        to: payerAccount.address,
        value: web3.utils.toWei(currentAmount.toString(), "ether"),
        gas: 21000,
        gasPrice,
      };

      const signedFundTx = await web3.eth.accounts.signTransaction(fundTx, generatedWallet.privateKey);
      await web3.eth.sendSignedTransaction(signedFundTx.rawTransaction);

      ctx.reply(`✅ Successfully sold ${currentQuantity} ${tokenDetails.symbol}!`);
    }
  } catch (error) {
    console.error("❌ Transaction Error:", error);
    ctx.reply(`❌ Transaction failed: ${error.message}`);
  }

  delete userSessions[chatId];
};

// Session management
const startSessionTimeout = (chatId) => {
  return setTimeout(() => {
    delete userSessions[chatId];
    console.log(`Session for user ${chatId} cleared due to inactivity.`);
  }, SESSION_TIMEOUT);
};

const resetSessionTimeout = (chatId) => {
  if (userSessions[chatId]) {
    clearTimeout(userSessions[chatId].timeoutId);
    userSessions[chatId].timeoutId = startSessionTimeout(chatId);
  }
};

// Start bot
bot.launch().then(() => console.log("✨ Trading Bot is live! 🚀"));
