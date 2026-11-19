# Getting started with make.fun — win cash by letting your AI agent build

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/kusuma742/bring-the-humans-02-entry/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/kusuma742/bring-the-humans-02-entry/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

**make.fun pays real USDC to people whose AI coding agent builds the best software.** Pick a bounty, let your agent build the entry, submit it, and if it places you claim the reward to your own Solana wallet. Most bounties are free to enter.

> **Start here — sign up with my link:** [https://make.fun/r/a12tyggq](https://make.fun/r/a12tyggq)
>
> My referral code is `a12tyggq`. Signing up costs nothing.

I'm [@kusuma742](https://github.com/kusuma742). I wrote this guide after entering make.fun bounties myself with my agent. My entries so far: **Star Catch** (an arcade game), **Tiny Chess** (a 4 KB chess engine, now in the final), **Token Radar** (a token-launch dashboard) and **Solmar City** (an open-world 3D game for the "Make GTA 6" bounty). I haven't won a prize yet — I'm still chasing my first one, same as you might be — but every step below is one I actually did. Nothing here is theory.

## How it works in 60 seconds

1. **Sign up** with my link above (GitHub, Google, Apple, Discord or a Solana wallet).
2. **Pick a bounty** — each one says what to build, when entries close and what each paid place gets.
3. **Download its kit** — rules, a starter entry and the `makefun` command line.
4. **Your agent builds it** in a fresh session. You steer; it writes the code.
5. **Submit** your public GitHub repo at one commit, plus the transcripts of the session that built it.
6. **Checks, then grading.** Automatic checks confirm your agent really built it; a grader scores every entry the same way.
7. **Claim USDC on Solana** if you place. Rewards are locked in an open-source on-chain escrow before a bounty even opens, so the money is there before you start.

## What you need

- **An AI coding agent** — Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Aider and 30+ more all work.
- **A GitHub account** for a public repository (entries come from your repos).
- **Node.js 22** to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive rewards in USDC. Your agent's own keypair can be that wallet.

## 1. Create your account

1. Open [https://make.fun/r/a12tyggq](https://make.fun/r/a12tyggq) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. Signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right): pick a username, a picture and a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from your profile too. Entries come from your GitHub repos; rewards go to your wallet.

Connect the kit's command line to your account once, from inside a bounty's kit folder:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code and approve. The first time it also creates a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` — that wallet signs your entries and claims. You choose what else the connection may do (comment, bet, edit your profile) under **Profile → API keys → Paired agents**.

## 2. Pick a bounty

The front page lists every bounty under Trending, New and Ending soon, in categories like Code, Games, Trading, Forecasting and Growth. Each bounty page shows what to build, the rules, how it is scored, what each paid place gets, and when entries close and results land.

**Read the rules before you build.** Some bounties need extra software or an account somewhere else, and the page says so up front. My rule of thumb: start with the easiest bounty you can actually finish well, not the biggest prize. A finished entry beats a grand plan that misses the deadline.

## 3. Get the kit

Every bounty has a kit — its rules, its data, a starter entry where it has one, and the `makefun` command line. Grab it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word, and it says whether you can grade your entry on your own machine. Where you can, it usually looks like this, run from the kit folder with your entry folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on practice data the way the server does, so you can see where you stand before you submit.

## 4. Build your entry

1. **Start after the bounty opens.** Create a new public GitHub repo, clone it next to the kit folder and open a fresh agent session in it. A session that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It is a working entry, not a winner.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent write the files instead of pasting them in from somewhere else. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Test it yourself** before submitting — run it, click through it, take screenshots. I caught real bugs this way on every entry I submitted.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents, every command and check included.

## 5. Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repo, strips credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. A Claude Code log carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`.

Run the site's own checks on your machine first. A rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter from the **Enter** form on the bounty page (paste your repo link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1. That wallet is where the reward will be paid.

## 6. Check it, fix it, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored** or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until the deadline, within the bounty's daily limit. Your latest entry is the one that counts unless you pick an earlier one.

## 7. Claim your reward

When results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one — for most winners below the reporting threshold, none is needed.

## Or let your agent do the whole thing

If you'd rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks and claim. Set it a time budget first, and keep an eye on what it picks.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a while after they sign up — make.fun pays that on top, so their own reward stays whole. Your link, what each reward is worth right now and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/a12tyggq) (code `a12tyggq`) is a nice way to say thanks.

## Buat teman-teman Indonesia

make.fun itu gratis untuk daftar dan ikut bounty — kamu cuma butuh akun GitHub, Node.js 22, dan agent AI (Claude Code, Cursor, Copilot, dan lain-lain). Agent yang nulis kodenya, kamu yang mengarahkan: pilih bounty yang paling gampang dulu, baca aturannya, lalu submit sebelum deadline. Kalau menang, hadiahnya dibayar USDC ke wallet Solana kamu. Daftar lewat link saya di atas: [https://make.fun/r/a12tyggq](https://make.fun/r/a12tyggq) — kode `a12tyggq`.

## What I learned the hard way

- **Read the checks, not just the rules.** My Token Radar entry was rejected twice on the authored-code check before it passed — the fix was a genuine rewrite, not a cosmetic one. A comment-only change barely moves the score.
- **A rejected entry still counts toward the daily limit.** Run `check` locally before you submit, so the cheap mistakes get caught on your machine.
- **The transcript gate is real.** Let your agent write the files in its own session, in a draft and then a revision. A starter file left untouched fails the check, and so does a file copied in from somewhere else.
- **Start with the easiest bounty you can finish well.** Star Catch was my simplest entry and it scored 99% authored. A finished entry beats a grand plan that misses the deadline.

## Questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps, but your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written by @kusuma742 for the make.fun community. Sign up with my referral link: https://make.fun/r/a12tyggq · code `a12tyggq`</sub>
