## SplitDo MVP: Direct SOL/USDC Transfer Model

### How It Works:
1. **User pays $10 via Stripe** 
2. **Stripe webhook triggers** → confirms successful payment
3. **SplitDo server** sends $9.99 worth of SOL (or USDC) from your **reserve wallet** directly to user's Solana pubkey
4. **You keep $0.01** as processing fee

### Technical Flow:
```
User Payment ($10) → Stripe → Webhook → Server → Transfer $9.99 SOL/USDC → User's Wallet
                                           ↓
                                    Keep $0.01 fee
```

### What We Need:

**1. SplitDo Reserve Wallet:**
- Hot wallet stored on your server (with private key)
- Pre-funded with SOL or USDC (the wallet gotta be pre-funded)
- Used to send funds to users after successful payments

**2. Stripe Integration:**
- Payment Intent for $10 charge
- Webhook endpoint to catch `payment_intent.succeeded`
- Link Stripe customer to Solana pubkey

**3. Solana Transaction Logic:**
- Calculate $9.99 worth of SOL (using current SOL/USD price)
- OR use USDC directly (simpler - 1:1 with USD)
- Send from reserve wallet to user's pubkey
- Log transaction for accounting

### Key Benefits of This Approach:
- **No token creation** - using existing Solana ecosystem
- **Simple accounting** - direct USD → SOL/USDC conversion  
- **Immediate liquidity** - users get real SOL/USDC they can use anywhere
- **Integration ready** - works with RedotPay, Kast, any Solana wallet

### Settlement Flow:
When users want to settle bills:
- They send SOL/USDC **from** their wallet **to** any other Solana pubkey
- You deduct $0.01 worth of SOL/USDC as fee during the transaction
- Settlement goes to recipient's pubkey (not back to SplitDo wallet)
