# Video script

**3:42.** Read the plain text. Do the bold lines at the same time.

---

## SETUP

Three terminals in `/Users/shaurya/Projects/Web3/RoadToDevcon`:

```bash
anvil --fork-url https://eth.drpc.org --port 8545 --chain-id 1
```
```bash
cd apps/evil-rpc && EVIL_UPSTREAM=https://eth.drpc.org ./node_modules/.bin/tsx src/index.ts
```
```bash
pnpm dev
```

Check the liar is alive:
```bash
curl -s -X POST -H 'content-type: application/json' --data '{"jsonrpc":"2.0","id":1,"method":"eth_chainId","params":[]}' http://127.0.0.1:8546 | grep -q '"0x1"' && echo OK || echo "BROKEN"
```

Browser: hide bookmarks (Cmd+Shift+B). Tab 1 = `localhost:5180/pitch.html`, Tab 2 = `localhost:5180/`. Load both fully before recording.

Copy this to your clipboard: `0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe`

**The console masthead must say `3 corroborations`.** If it says 2, the liar isn't running and Beat 4 has nothing to reject.

---

# 0:00

**Tab 1, top of page. Don't move the mouse.**

Okay so before showing our product I would like you to 
Open your MetaMask. It says you've got some amount of ETH in there.

But have you ever thought, Where did that number actually come from?

So it basically came from Infura which is metamsk's RPC node service. Your wallet asked for the balance, Infura sent back a number, MetaMask printed it. That's it. There was no proof anywhere in that.

So if Infura sends the wrong number, your wallet has no way to know. It just shows it to you.

---

# 0:21

**Circle the panel on the right once with your mouse.**

Here's the thing though. Ethereum can actually prove balances. There's an RPC method, eth_getProof, which hasbeen sitting there since like 2018. Almost nothing uses it.

So we built something that does. This is Nullius.

---

# 0:35

**Click the address box, select all, paste, hit Verify. Point at the green PROVEN, then the yellow diagram.**

SO now Let me show you. Any address works, this one's the Ethereum Foundation's.

It doesn't ask for the balance. It asks for the proof, and gets nine trie nodes back. My browser hashes every one of them, checks each against its parent, straight up to the state root in the block header.

Nine nodes, three milliseconds. That number's proven now. Not trusted. Proven.

---

# 1:00

**Point at the red block underneath. Click "Show the forged proof breaking". Point at row 04.**

Okay so what happened is we basically wrote our own malicious RPC. It like multiplies every balance by a hundred, and corrupts one node inside every proof.

so Same account, through the liar it shows and Your wallet would have just shown you that.

But Ours doesn't.It was Rejected, and it also showed where it was changed, at node four.

And thre is no way to get around this. To fake a proof that actually passes, you'd practically have to break keccak256 itself.

---

# 1:32

**Scroll down to the comparison table. Leave it on screen.**

Quick thing, because this is the real argument.

Most people solve this by querying five RPCs and taking the majority. That's a vote. Corrupt three, you own the answer.

A proof doesn't care how many agree. One honest node out of any number is enough. And if all of them lie to us, we show you nothing instead of showing you garbage.

---

# 1:52

**Switch to Tab 2. Point at the small square marks in the register, then sweep across the top bar.**

---

# 2:12

**Click II · THE ROUTE on the right rail. Keep talking ~5 seconds while it loads. Then point at the relay table, then the recorder, then the route saying "no reply".**

So Your transaction doesn't go straight to a validator. It goes through builders and relays, and some of those relays filter transactions.

So we pull the relays' own bid trace feeds and count who's delivering what. This is live.

Then one transaction, with three routes at the same time. We show that one took it. Second one bounced it back. And the third one, nothing at all. It just swallowed it.

That third one is the whole point. That's what verification actually looks like from your side.

---

# 2:42

**Click III · THE QUORUM. Point at the panel bottom right. Click "for". Then hover the buttons to show they're dead.**


So We wrote a circuit in Noir. It proves you control an account holding real ETH, and outputs a nullifier. A one way tag.

So I vote once. Try again and it's rejected, because that nullifier's already spent. No KYC, no orb, no phone number. Sybil resistance that costs ETH instead of privacy.

---

# 3:06

**Back to Tab 1. Stop on the light coloured architecture diagram.**

So why does any of this matter?

If you're an activist receiving funds somewhere that doesn't want you to, you need to know your transaction got out. Not assume it.

If you're about to sign something, you should know the balance is real.

And a DAO shouldn't need your passport before it lets you vote.

---

# 3:26

**Scroll to the bottom. Stop moving the mouse.**

Everything you just watched ran in the browser, against mainnet, with a Merkle Patricia verifier we wrote ourselves. No library.

It's live, link's below. Go put your own address in it.

Don't trust me on any of this. Go check it.

**Stop recording.**

---

## IF IT BREAKS

| You see | Do |
|---|---|
| Masthead says 2 corroborations | `lsof -ti:8546 \| xargs kill -9`, restart terminal 2 |
| A value shows a dash | Same as above |
| Relay table all dashes | Network dropped. Skip "this is live", carry on |
| Broadcast errors | Restart terminal 1, reload the page |
| Blank page | Restart `pnpm dev` |
