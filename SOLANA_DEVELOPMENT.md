# Solana Development Guide

## Anchor Framework

**Overview:** Rust-based framework for Solana smart contracts (programs).

### Setup

```bash
# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install Solana CLI
sh -c "$(curl -sSfL https://release.solana.com/stable/install)"

# Install Anchor
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force
avm install latest
avm use latest

# Verify installation
anchor --version
```

### Create Project

```bash
# Create new Anchor project
anchor init my-project
cd my-project

# Project structure:
# programs/       - Rust programs (smart contracts)
# tests/          - TypeScript tests
# migrations/     - Deployment scripts
# Anchor.toml     - Configuration
```

### Write Smart Contract

```rust
// programs/my-project/src/lib.rs
use anchor_lang::prelude::*;

declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");

#[program]
pub mod my_project {
    use super::*;

    pub fn initialize(ctx: Context<Initialize>) -> Result<()> {
        let my_account = &mut ctx.accounts.my_account;
        my_account.data = 0;
        Ok(())
    }

    pub fn update(ctx: Context<Update>, new_data: u64) -> Result<()> {
        let my_account = &mut ctx.accounts.my_account;
        my_account.data = new_data;
        Ok(())
    }
}

#[derive(Accounts)]
pub struct Initialize<'info> {
    #[account(init, payer = user, space = 8 + 8)]
    pub my_account: Account<'info, MyAccount>,
    #[account(mut)]
    pub user: Signer<'info>,
    pub system_program: Program<'info, System>,
}

#[derive(Accounts)]
pub struct Update<'info> {
    #[account(mut)]
    pub my_account: Account<'info, MyAccount>,
}

##[account]
pub struct MyAccount {
    pub data: u64,
}
```

### Test

```typescript
// tests/my-project.ts
import * as anchor from "@coral-xyz/anchor";
import { Program } from "@coral-xyz/anchor";
import { MyProject } from "../target/types/my_project";

describe("my-project", () => {
  const provider = anchor.AnchorProvider.env();
  anchor.setProvider(provider);

  const program = anchor.workspace.MyProject as Program<MyProject>;

  it("Initializes account", async () => {
    const myAccount = anchor.web3.Keypair.generate();

    await program.methods
      .initialize()
      .accounts({
        myAccount: myAccount.publicKey,
        user: provider.wallet.publicKey,
        systemProgram: anchor.web3.SystemProgram.programId,
      })
      .signers([myAccount])
      .rpc();

    const account = await program.account.myAccount.fetch(myAccount.publicKey);
    assert.equal(account.data.toNumber(), 0);
  });
});
```

### Build & Deploy

```bash
# Build
anchor build

# Test
anchor test

# Deploy to devnet
solana config set --url devnet
anchor deploy

# Deploy to mainnet
solana config set --url mainnet-beta
anchor deploy
```

## Frontend Integration

```typescript
import { Connection, PublicKey } from '@solana/web3.js';
import { Program, AnchorProvider } from '@coral-xyz/anchor';

// Connect to cluster
const connection = new Connection('https://api.mainnet-beta.solana.com');

// Initialize provider
const provider = new AnchorProvider(connection, wallet, {});

// Load program
const programId = new PublicKey('YOUR_PROGRAM_ID');
const program = new Program(idl, programId, provider);

// Call program method
await program.methods
  .update(new anchor.BN(42))
  .accounts({
    myAccount: accountPublicKey,
  })
  .rpc();
```

## Resources

- **Anchor Docs:** https://www.anchor-lang.com
- **Solana Cookbook:** https://solanacookbook.com
- **Solana Docs:** https://docs.solana.com
