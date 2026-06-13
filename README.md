# SHROUDED

A multiplayer social-deduction game in the style of *Among Us*, written from
scratch in C using SDL2. Up to **6 players** join the same game over a local
network. One player is secretly the **Killer**; everyone else is an innocent
**Crewmate**. Crewmates race to finish their tasks or vote the Killer out — the
Killer tries to eliminate enough Crewmates to take over.

---

## What's in this folder

**Keep every file in this folder together — do not move anything into
subfolders.** Windows looks for the game's libraries (the `.dll` files) in the
same folder as the `.exe`, and the game looks for `assets/` right next to the
`.exe`. If you separate them, the game will not start.

```
client.exe        ← every player runs this
server.exe        ← ONE person runs this to host the game
assets/           ← game images, sounds, and fonts (keep next to the .exe)
*.dll             ← ~50 library files — ALL required, leave them all here
README.md         ← this file
```

You can move or copy the **whole folder** anywhere you like (desktop, USB stick,
another PC) as long as its contents stay together. You do **not** need MSYS2,
SDL, or anything else installed — every library the game needs is already in
this folder.

---

## Requirements

- Windows (64-bit).
- All players must be on the **same network** (same Wi-Fi or the same LAN).
- One machine acts as the host by running `server.exe`.

---

## First time you run it: "Windows protected your PC"

Because this is a small game that isn't signed by a big publisher, Windows may
show a blue box reading **"Windows protected your PC"** the first time you launch
`server.exe` or `client.exe`. This is normal and does **not** mean anything is
wrong with the game.

To run it: click **"More info"**, then click the **"Run anyway"** button that
appears. You only need to do this once per `.exe`.

If your antivirus instead *removes* an `.exe` (the file disappears from the
folder), restore it from your antivirus's "protection history" or quarantine and
allow it, or add this folder as an exclusion. A from-scratch game that opens
network connections is a common false-positive — the game does not contain
malware.

---

## How to run the game

### Step 1 — Start the server (host only)

One person double-clicks **`server.exe`**. A console window opens and shows:

```
Server listening on port 2000...
```

Leave this window open for the whole session. Closing it ends the game for
everyone.

The host also needs to find their **local IP address** so others can connect.
See the next section for how to do that.

### Step 2 — Join as a player (everyone, including the host)

1. Double-click **`client.exe`**.
2. On the main menu, choose to start.
3. When prompted for the server address, type the host's IP (e.g.
   `192.168.1.42`) and connect.
4. You'll land in the **lobby**. Wait for everyone to join.

### Step 3 — Start the match

The **host** (the first player who joined) starts the game by pressing
**`SPACE`** in the lobby once **at least 2 players** have joined. Everyone then
sees their secret role, and the round begins.

---

## Finding the host's IP address

Only the **host** (the person running `server.exe`) needs to do this. The IP is
the address other players type in when they connect.

### Quick method (Command Prompt)

1. Press `Win + R`, type `cmd`, and press Enter.
2. Type `ipconfig` and press Enter.
3. Find the section for the connection you're actually using — **Wireless LAN
   adapter Wi-Fi** if you're on Wi-Fi, or **Ethernet adapter** if you're plugged
   in with a cable.
4. Read the line that says **IPv4 Address**. It looks like:

   ```
   IPv4 Address. . . . . . . . . . . : 192.168.1.42
   ```

5. That number (`192.168.1.42` in this example) is what the other players type
   in. Share it with them.

### Alternative method (Settings menu)

1. Open **Settings → Network & Internet**.
2. Click your active connection (**Wi-Fi** or **Ethernet**).
3. Scroll to **IPv4 address** — that's the number to share.

### Tips for reading the result

- A home/office IP almost always starts with **`192.168.`** or **`10.`**.
- **Ignore** any address that says `127.0.0.1` — that only works on the host's
  own machine, not for other players.
- If `ipconfig` lists several adapters, pick the one for the network everyone is
  actually on. Virtual adapters (VirtualBox, VMware, Hyper-V) and disconnected
  adapters can be ignored.
- The IP can change when you reconnect to the network or restart, so check it
  again at the start of each session.

### Everyone on the same computer (testing)

If you just want to test the game with multiple windows on one machine, players
can use **`127.0.0.1`** instead of hunting for the network IP.

> Players don't need to find any IP — only the host does. Everyone else simply
> types in the host's address.

---

## How to play

### Your goal

- **Crewmates** win by either completing **all of their tasks** or by **voting
  out the Killer** in a meeting.
- The **Killer** wins by eliminating Crewmates until the Killer is **equal to or
  greater in number** than the remaining Crewmates.

### Roles

At the start of each round you're shown your role:

- **Innocent (Crewmate)** — walk around the map and complete your assigned
  tasks. You can call meetings and report bodies, but you cannot kill.
- **Killer (Impostor)** — blend in, and eliminate Crewmates when no one is
  looking. After each kill there is a **30-second cooldown** before you can kill
  again.

### Controls

| Key / Action | What it does |
|---|---|
| `W` `A` `S` `D` | Move your character |
| `E` | Interact — start a task when standing on a task spot; open the emergency-meeting screen when standing on the meeting table |
| `K` *(Killer only)* | Kill the nearest valid target in front of you (or click the kill button) |
| `R` | Report a dead body when you're standing near one |
| `M` | Open / close the task map |
| `TAB` | Show / hide the task list panel |
| `I` | Show / hide the controls overlay |
| `Q` | Cancel / leave a task you've opened |
| `ESC` | Open the pause menu |
| Mouse | Click buttons (kill, report, meeting, vote) |

### Tasks (Crewmates)

Walk onto a task spot and press `E` to open the mini-game for that task. Finish
it to mark the task complete. When **all Crewmates finish all their tasks**, the
Crewmates win — so keep moving and getting them done.

### Meetings and voting

A meeting starts when someone **reports a body** (`R` near a corpse) or **calls
an emergency meeting** (stand on the meeting table, press `E`, then click the
button). During a meeting:

- Everyone gets a short time to discuss.
- You then have a **60-second** voting window.
- Click a player's banner to choose who you think the Killer is, then submit
  your vote. You can also choose to **skip**.
- The player with the most votes is eliminated. A tie (or skip winning) means
  no one is removed.

After the result is shown, survivors return to the map and play continues.

### Winning and playing again

When a round ends you'll see a win screen. From there you can choose **Play
Again** to start a fresh round with new roles, or return to the main menu.

---

## Troubleshooting

**The game won't open / "missing DLL" error.**
Make sure **all** the `.dll` files are in the same folder as the `.exe` — there
are around 50 of them and every one is needed. Don't move the `.exe` out on its
own, and don't delete any `.dll` to "tidy up."

**Black screen or instantly closes.**
The `assets/` folder must be right next to the `.exe`, with its contents intact.
Don't rename or move it.

**Can't connect to the server.**
- Check the IP address is typed correctly.
- Make sure everyone is on the same network.
- Confirm the host's `server.exe` window is still open and shows
  "Server listening".
- A firewall may be blocking the connection — allow the game through Windows
  Firewall, or temporarily disable it to test. The game uses **ports 2000 and
  2001**.

**Pressing SPACE doesn't start the game.**
Only the **host** (first player to join) can start, and you need at least
**2 players** in the lobby.

---

## A note on the network design

Shrouded uses a hybrid networking model: fast, frequent player movement is sent
over **UDP** (where an occasional dropped packet doesn't matter, since the next
update overwrites it), while critical, must-not-be-lost events — kills, votes,
meeting triggers, and phase changes — are sent over a reliable **TCP** channel.
The server is authoritative: it validates kills, vote eligibility, and body
reports rather than trusting the clients, which prevents basic cheating.

Have fun, and trust no one.
