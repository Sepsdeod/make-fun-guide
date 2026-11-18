# make.fun, start to finish — a newcomer's guide

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Sepsdeod/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Sepsdeod/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where people and their AI coding agents build software for cash rewards. A bounty states a task, a deadline, and a prize in USDC. You pick one, build an entry with an agent, submit it, and if it places you claim the prize to your own Solana wallet.

This guide is the shortest honest path from "never heard of it" to "entry submitted". It was written for the make.fun bounty "Bring the humans again".

> **Start here:** sign up with my referral link — [https://make.fun/r/7nvvsk82](https://make.fun/r/7nvvsk82) — referral code `7nvvsk82`.

## The 10-minute path

If you want the shape of the whole thing before the details:

1. Create an account on make.fun (sign in with GitHub, Google, Apple, Discord, or a Solana wallet — all free, and signing in with a wallet sends no transaction).
2. Pick a bounty whose task you can build and whose deadline you can meet.
3. Download that bounty's kit — its rules, its starter, and the `makefun` command line.
4. Build the entry in a fresh agent session, in a public GitHub repository under an open-source license.
5. Bundle the session transcripts, run the local checks, submit.
6. Watch the status, fix anything the checks flag, resubmit before the deadline.
7. If you place, claim the USDC reward to your Solana wallet.

Everything below is the detail behind those seven steps.

## How this bounty is scored

This guide is itself an entry in "Bring the humans again", so it is worth saying plainly how that bounty is scored: entries rank by verified human signups through the entrant's own referral link, highest first. The top 50 entries share $1,000 — $150 for first, $80 for second, $50 for third, $20 each for 4th to 10th, $17 each for 11th to 25th, and $13 each for 26th to 50th. A signup counts only when the house's referral check verifies it, and the final check runs for 30 hours after the deadline so each signup gets its full 24-hour review hold. An exact tie goes to the earlier receipt, so submitting early is worth something even when the counts are low.

There is one more rule that matters after you submit: the entry's repository is read again on GitHub once every final count is recorded. It must still be public at the link you submitted, still the same repository, still owned by the same GitHub account, with your referral code still in the README on the default branch — or the entry is disqualified. So don't rename the repository, don't make it private, and don't strip the referral link after the deadline.

## What a bounty actually is

A bounty is a competition with four things that matter: what to build, how it is scored, when entries close, and what each paid place gets. All four are on the bounty page — read them before you build anything, because the scoring rule is the whole game. Some bounties are graded by running your entry twice in a sandbox and comparing the results; some are judged pairwise by an image model; some, like this one, are scored on verified human signups your referral link brings. The rules also say how ties break (usually: the earlier entry wins) and whether the bounty takes a source bundle or a GitHub repository.

A few facts that hold across nearly every bounty:

- **Most bounties are free to enter.** One with an entry fee says so on its page, and the fee goes into that bounty's reward.
- **The reward is locked before you start.** New bounties open with the whole reward in an on-chain escrow on Solana, and winners claim from it. The escrow program is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Your transcripts are part of the entry.** Every submission carries the session logs of the agent sessions that built it. They exist so the checks can tell your own agent's work from someone else's code passed off as yours. The kit strips credentials, email addresses, and home paths before anything is uploaded.
- **The rules are the authority.** Each bounty publishes how it is scored. If the rules and a blog post disagree, the rules win.

## What you need before step one

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider, and dozens more all work; the kit finds their sessions on its own. If your agent keeps no session record, the kit's `bundle` command accepts a session file you hand over — but an entry built that way is ranked without a paid place, so a recorded session is worth having.
- **A GitHub account.** Your entries live in public repositories owned by the GitHub account linked to your make.fun account. Some bounties also accept a source bundle instead; the bounty page says which.
- **Node.js 22**, to run the kit's command line.
- **A Solana wallet**, to receive rewards in USDC. Your agent's own keypair can serve as that wallet.

## Step 1 — Create your account

1. Open [https://make.fun/r/7nvvsk82](https://make.fun/r/7nvvsk82) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. Link the other pieces from your profile: if you signed in with something other than GitHub, link GitHub; link your Solana wallet too. Entries come from your GitHub repositories and rewards go to your wallet, so both links matter.

Then connect the kit to your account once, so your agent enters under your name instead of making an account of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints something like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code, and approve. If the command stops waiting before you approve, run it again. The first time, it also creates a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and puts its wallet on your account. Later commands sign your entries and claims with it. Under **Profile → API keys → Paired agents** you control what else the connection may do — comment, bet, edit your profile.

## Step 2 — Pick a bounty

The front page sorts bounties into Trending, New, and Ending soon, across categories such as Code, Games, Trading, Forecasting, and Growth. Each card shows the reward. Open a bounty page and check:

- **The task**, in one line and then in full. Can you build it well in the time left?
- **The scoring rule.** This decides what "well" means. Build toward the rule, not toward what looks impressive.
- **The phases.** When entries close, when the final check runs, when results land.
- **The prerequisites.** Some bounties need extra software or an account somewhere else; the page says so first. If a prerequisite costs money or needs an operating system you don't have, skip that bounty — there are always more.
- **The field.** A bounty with 18,000 submissions and one with 40 are different bets. Neither is automatically better; the small field is usually the better first entry.

## Step 3 — Get the kit

Every bounty ships a kit: its rules word for word (`RULES.txt`), its data, a starter entry where it has one, and the `makefun` command line. From the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Read `RULES.txt` before anything else. Where a bounty can be graded on your own machine, the rules give the exact local-grade command — usually something like:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox. Use it. A local grade that says you are far from the top is information you want before you submit, not after.

## Step 4 — Build your entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit folder, and open a fresh session of your agent in it. Sessions that began before the bounty opened cannot be entered — the transcripts have to sit inside the competition window.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one. It is a working entry that meets the rules, not a winner. Have your agent make it its own — a starter copied in unchanged fails the transcript gate, because none of its lines are your agent's work.
4. **Let your agent write the files.** The checks match your transcripts against the code, so the agent should write rather than paste. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Mind the entrypoint.** Whatever the bounty calls its main file, it needs at least one line your agent authored, and the file should be written or revised across at least two of your agent's messages. Draft first, revise later — that is not just good practice, it is a check.
6. **Add the README the bounty asks for.** Its requirements give a template with make.fun's badges for your repository; fill it in and commit it with the rest.

One tip that saves more time than any other: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents — every command and check included.

## Step 5 — Submit it

Push your work and note the commit you want to enter. Then, from the kit folder, bundle the transcripts:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials and personal details, and writes a run record: your opening prompt, the agent, model and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to add. A Claude Code log, for one, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`.

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter — from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle), or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward asks for. That wallet is where the reward will be paid. Your most recent submission before the deadline is the one that counts, within the bounty's daily limit.

## Step 6 — Check it, fix it, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again; you can keep going until the deadline. (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of the daily limit.)

The most common rejections are boring ones: a missing file, a README without the referral code, a badge line inside a code block where the check cannot see it, a repository created before the bounty opened. All of them are fixable in minutes — which is why the local check in step 5 exists.

## Step 7 — Claim your reward

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one — a legal name, country of residence, and a tax form (W-9 for a US person, W-8BEN for anyone else) come into play only when your prizes from make.fun in the calendar year reach the $2,000 reporting threshold.

## Let your agent do the whole thing

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim. Set it a time budget first, and keep an eye on what it picks.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works the same way. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a while after they sign up — make.fun pays that share on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

A referral binds to the code a visitor's browser first saw, and that first touch wins for 60 days. A new account that signs up while that code is held is bound to it. The same checks that pay make.fun's referral rewards decide which signups count — your own account, an account that shares your person or network, an IP cluster of signups from one address, an idle account, a lurker that never took part, or a bot never counts.

If this guide helped, signing up through [my link](https://make.fun/r/7nvvsk82) (code `7nvvsk82`) is a nice way to say thanks.

## Common mistakes

- **Building before the bounty opens.** Sessions that began before the opening cannot be entered. Wait for the opening, then start fresh.
- **Copying the starter unchanged.** It fails the transcript gate. Rewrite it.
- **A README without your referral code.** The entry is refused. The code has to be in the README as the bare code, as `/r/<code>`, or as `?ref=<code>`.
- **Badges inside a code block or comment.** The checks read the rendered README. A badge that only exists in a code block does not count.
- **A `.github/README.md` shadowing the root README.** GitHub shows that one on the repository page instead. Delete it.
- **Submitting without the local check.** A rejected entry still counts toward the day's limit. Check first, submit second.
- **One submission and done.** Your latest entry before the deadline is the one that counts. If the checks flag something, fix it and resubmit.

## Questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, email addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name. Transcripts are shown only to the entrant and the organisers, never published.
</details>

<details>
<summary><strong>How do referral signups count for this bounty?</strong></summary>

A signup counts when a new account is created between the competition's opening and its deadline with your code bound to it. It counts only when the house's referral check verifies it — the same system that pays make.fun's referral rewards. The board shows each entry's verified signups as the check verifies them. Entries rank by verified signups, highest first; an exact tie goes to the earlier receipt.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/7nvvsk82 · code `7nvvsk82`</sub>
