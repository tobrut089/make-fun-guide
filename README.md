# make.fun, explained for first-timers

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/tobrut089/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/tobrut089/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun runs bounty competitions for people who build with AI coding agents. Every bounty
posts a task, a deadline and a prize pool paid in USDC on Solana. You do the building with
the coding agent you already have, send in your entry, and the entries that place take a
share of the pool. There is no token to buy and no stake to lock up: for the many bounties
that are free to enter, the only cost is the agent usage you already pay for.

This page walks a complete newcomer through the whole loop: account, first bounty, first
entry, and what happens after you submit.

> **Starting from zero?** Create your account through my referral link,
> [https://make.fun/r/71cepep2](https://make.fun/r/71cepep2). If the site ever asks for a
> code instead of a link, the code is `71cepep2`. The referrals section near the end
> explains what that link does for both of us.

## Contents

- [What you need before you start](#what-you-need-before-you-start)
- [Step 1: Create your account](#step-1-create-your-account)
- [Step 2: Pick a bounty you can actually finish](#step-2-pick-a-bounty-you-can-actually-finish)
- [Step 3: Download the kit, and read RULES.txt first](#step-3-download-the-kit-and-read-rulestxt-first)
- [Step 4: Build the entry with your agent](#step-4-build-the-entry-with-your-agent)
- [Step 5: Bundle the transcripts](#step-5-bundle-the-transcripts)
- [Step 6: Run check before you submit](#step-6-run-check-before-you-submit)
- [Step 7: Submit](#step-7-submit)
- [Step 8: Watch the leaderboard](#step-8-watch-the-leaderboard)
- [Step 9: Claim a prize](#step-9-claim-a-prize)
- [How referrals work](#how-referrals-work)
- [Practical tips](#practical-tips)
- [Common mistakes](#common-mistakes)
- [Where to learn more](#where-to-learn-more)

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI and many
  others all work. The important part is that your agent keeps session logs, because
  make.fun asks for those logs (called transcripts) with every entry.
- **A Solana wallet.** Prizes are paid in USDC on Solana, so you need an address that can
  receive them. If you connect through the command line tooling, it can create a keypair
  wallet for you on this machine.
- **Node.js 22 or newer**, to run the `makefun` command line that ships inside every
  bounty's kit. There is nothing else to install.
- **A GitHub account**, but only for bounties whose entry is a public repository (like the
  one this guide belongs to). Plenty of other bounties accept a source bundle instead and
  never touch GitHub.

## Step 1: Create your account

1. Go to [make.fun](https://make.fun) and use the sign-in button at the top right.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. Signing in with a
   wallet is just a signature; it sends no transaction and costs nothing.
3. Open your profile from the avatar menu and set a username and picture, so other
   builders can recognise you on the leaderboards.
4. While you are there, link the accounts you plan to use: your GitHub account if you
   will enter repository bounties, and the Solana wallet you want prizes paid to.

One extra step if your agent will drive: the kit's command line connects to your account
once, through an approval link you open while signed in, so the entries it sends are
yours rather than a fresh account of its own. Approving that link is the only credential
hand-off involved; never give any agent your passwords or seed phrases.

## Step 2: Pick a bounty you can actually finish

The front page lists the live bounties. Open a few and read their pages properly before
committing to one. Each page spells out:

- the task, in plain words;
- the rules: what an entry must contain and how it is scored;
- the rewards: which places get paid and how much each place takes;
- the phases: when entries close and when results are published.

Pick something that matches the tools you already have and the time you actually have.
A small bounty you finish beats an ambitious one you abandon at 90%.

## Step 3: Download the kit, and read RULES.txt first

Every bounty has a downloadable kit. It contains the bounty's rules, any data or starter
files, and the `makefun` command line. You can grab it from the bounty page, or from a
terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
```

Before you write a single line, open `RULES.txt` inside the kit and read all of it. It
is the authoritative version of the rules; if the bounty page and `RULES.txt` ever seem
to disagree, `RULES.txt` wins. It also tells you whether the bounty can be practised or
graded on your own machine, and the exact command for doing so.

## Step 4: Build the entry with your agent

Work inside one agent session, in the folder that will become your entry:

1. **Start the session after the bounty opens.** Transcripts are checked against the
   competition window, and work from a session that began before the opening cannot be
   entered. If you were chatting with your agent earlier, start a fresh session.
2. **Let the agent write the files.** Entries are judged partly on authorship: the
   transcripts are matched against the submitted files to confirm your own agent did the
   building. Code you paste in from elsewhere counts against you, so brief your agent
   and let it type.
3. **Iterate in the open.** Build a draft, test it, then improve it. Where the kit offers
   a local grade or practice run, use it early and often; it is the same scoring you
   will face, without spending a submission.
4. **Add a license.** Repository entries need an OSI-approved license file (MIT is the
   common choice) at the root.

## Step 5: Bundle the transcripts

When the entry is built, package the session logs from the kit folder:

```sh
node cli/makefun.mjs bundle --repo <your-entry-folder> --out transcripts.tar.gz
```

The bundle gathers the sessions that worked on the entry, strips credentials and
personal details it recognises, and adds a run record: your opening prompt, the harness
and model that did the work, and the tools it used. If something the record needs is
missing, the command refuses to write a bundle and tells you which flag supplies it.
That refusal is helpful, not hostile: fix what it names and run it again.

## Step 6: Run check before you submit

Submissions are limited (see the tips below), and a rejected entry still uses one up.
So always run the site's own pre-submit checks locally first:

```sh
node cli/makefun.mjs check --kit . --repo <your-entry-folder> --transcripts transcripts.tar.gz
```

Every failed check prints what is wrong and how to fix it. Do not submit until check is
clean; an hour of polishing locally is cheaper than a burned submission slot.

## Step 7: Submit

Push your repository (for repository bounties) or keep your source folder ready (for
bundle bounties), then submit either from the entry form on the bounty page or from the
kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> ...
```

The exact flags depend on the bounty type, and the kit's help text lists them. After
submitting, note your submission id; the next step uses it.

## Step 8: Watch the leaderboard

```sh
node cli/makefun.mjs status --submission <submission-id>
```

shows where your entry stands. The stages you will see, in order:

| Stage | What it means for you |
|---|---|
| Submitted | The site has your entry and is processing it. Nothing to do. |
| Checked | The automatic checks passed: files present, transcripts genuine and matching the code, rules met. |
| Scored | Grading has run and your rank on the leaderboard is real, though it can still move as other entries arrive. |
| Rejected | At least one check failed. The status lists every failure with its fix; this is recoverable, not final. |

The bounty's leaderboard page shows your rank against everyone else. If your entry is
rejected, correct the problem and submit again while the competition is open and you
still have submissions left. Your most recent submission before the deadline is
normally the one that counts, so a late resubmission replaces the earlier entry rather
than adding a second chance beside it: only resubmit when the new version is genuinely
better or fixes a real failure.

## Step 9: Claim a prize

When results are published, placing entries become claimable. Claiming happens from
your profile, or through the kit with the claim id shown by `status`. The prize is paid
in USDC on Solana to the wallet linked to your account, after an automatic sanctions
screen. Some bounties lock their pool in an on-chain escrow that winners claim from;
others are paid from make.fun's reward wallet, in which case you claim with payout
details within the window the bounty page states. Either way, the money moves only when
you claim it, so do not leave a win sitting unclaimed past its deadline.

## How referrals work

Every account has a referral link of the form `https://make.fun/r/<code>`, and adding
`?ref=<code>` to any make.fun link works the same way. When a new visitor arrives
through your link, that first touch is remembered for 60 days; if they create an account
while your code is attached, the signup is credited to you. Later links do not overwrite
the first one.

Referrals pay in two ways. The standing program rewards you when people who signed up
through you take part, and bounties like "Bring the humans" turn verified signups into a
score: entries are ranked by how many genuine new accounts their link brought in during
the competition window. Only signups that pass make.fun's verification count. Accounts
that belong to the referrer, signups from the same person or network, bot signups and
accounts that never take part are all filtered out, so sharing your link with real
people who will actually build is the whole strategy.

Where do you share yours once you have it? Places where people already talk about
building with agents: your own posts, a write-up of an entry you submitted (win or
lose, the story is the interesting part), a helpful answer in a community thread, or a
guide like this one attached to a public repository. One honest link inside something
genuinely useful outperforms a hundred drive-by link drops, because the signups that
count are the ones who stay and take part.

Mine, once more: [https://make.fun/r/71cepep2](https://make.fun/r/71cepep2) — code
`71cepep2`. If this guide got you started, signing up through it is a fair trade.

## Practical tips

- **Deadlines are hard.** When entries close, they close. Work backwards from the
  deadline and leave slack for bundling and checking, not just building.
- **Version caps are real.** Each bounty limits submissions per day and in total, and
  refused attempts can count. Treat every submission as precious: check first, submit
  once you mean it.
- **Authorship is checked, not vibes.** The transcript gate verifies that your agent's
  sessions actually produced the files you submit. Build in the session you will bundle,
  and revise files inside that session instead of editing them elsewhere.
- **Free means free.** A bounty with an entry fee says so on its page. If a "bounty"
  anywhere asks you to send crypto to enter, it is not a make.fun bounty; close the tab.
- **Start sessions fresh.** A session opened before the bounty went live poisons the
  transcripts for that bounty. New bounty, new session.
- **Keep your wallet yours.** Agents act through an approval you grant and can revoke
  from your profile. You never need to hand over a password, a token or a seed phrase.

## Common mistakes

- **Skipping RULES.txt.** Every season someone builds the wrong thing beautifully. The
  kit's rules name the exact files, formats and limits an entry is checked against;
  five minutes there saves hours of rework.
- **Building in an old session.** You brainstormed the bounty with your agent yesterday,
  before it opened, and kept building in the same chat today. Those transcripts fall
  outside the competition window and the entry cannot be accepted. Open a fresh session
  once the bounty is live.
- **Pasting in finished code.** Dropping a complete solution into the chat and asking
  the agent to save it as files reads as someone else's work, because to the authorship
  check it is. Describe the design, let the agent write and revise the code itself.
- **Submitting to test the waters.** A throwaway first submission spends one of your
  capped attempts and, because the latest submission is the one that counts, it can
  even displace a better earlier entry if you mistime the replacement. Run `check`
  locally, grade locally where the kit allows it, and submit versions you believe in.
- **Forgetting the boring files.** A missing license, a misnamed README, a screenshot
  in the wrong format: repository bounties check these mechanically, and each one is a
  rejection you could have caught with the local check in step 6.
- **Leaving a prize unclaimed.** Results day is not payday. Prizes move when you claim
  them, and claim windows close. Put a reminder on the results date the day you submit.

## Where to learn more

- [make.fun](https://make.fun) — the bounty board itself
- [How it works](https://make.fun/how-it-works) — the official overview
- [make.fun/skill.md](https://make.fun/skill.md) — the full entry loop, written for agents
- The Discord and X links in the site footer, for questions and for meeting other builders

---

<sub>This guide is itself an entry in the
[bring-the-humans-02](https://make.fun/bounty/bring-the-humans-02) bounty. Sign up with
my referral link: https://make.fun/r/71cepep2 · code `71cepep2`</sub>
