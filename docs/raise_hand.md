# Raise Hand System (AUTO)

The **Raise Hand** system is AUTO’s way of making large live learning sessions fair, engaging, and manageable. Instead of chaotic chat scrolls, it gives every participant a clear, structured way to interact with the Scholar.

---

## 🎯 Goals
- Provide fairness in large groups (random selection, no bias).
- Reduce chat chaos by organizing questions.
- Allow asynchronous participation after live sessions.
- Create opportunities for scholars to monetize through engagement.

---

## 🖥 Scholar’s Interface
- **Room Count** → shows number of users connected.
- **Hands Raised Counter** → displays how many users are in queue.
- **Pick Question (Green Button)** → randomly selects a participant.
- **Timer** → configurable (e.g., 30–60 seconds).
- **Async Inbox** → holds questions left for after the session.
- **Moderation Tools** → mute, skip, or extend cooldown.

---

## 👤 User’s Interface
- **Raise Hand Button** → tap to enter the queue.
- **Cooldown** → 10 minutes between raises (configurable).
- **Keep Hand Raised** → option to submit question asynchronously after live session ends.
- **Question Submission** → text, audio, or short video with optional tags/donation.
- **Notifications** → user is alerted if chosen or when their question is answered.

---

## ⚖️ Fairness & Spam Protection
- **Random default** → unbiased participant selection.
- **First-Come-First-Served option** → available toggle for scholars.
- **Priority boost** → users who haven’t spoken in the session get slight weighting.
- **Spam guard** → one raised hand per user, per session; cooldown enforced.

---

## 🔄 Async Q&A (Keep Hand Raised)
- Users can leave their hand raised to submit a question for the scholar after live ends.
- Scholars can respond later with text, audio, or short video.
- Async answers generate **evergreen Q&A threads** searchable within AUTO.
- Optional donation button appears alongside async questions.

---

## 💰 Monetization Hooks
- **Live donations** → gift/donate button on the active question card.
- **Async donations** → donations tied to submitted questions.
- **Perks (optional)**:
  - Priority boost in queue for donors (transparent and capped).
  - Thank-you badges for contributors.
  - Optional supporter wall for recognition.

---

## 📊 Data Model (High-Level)

**rooms**
- `id`, `scholar_id`, `title`, `is_live`, `cooldown_minutes`, `created_at`

**hands**
- `id`, `room_id`, `user_id`, `status (raised|selected|answered|lowered|async_open)`, `mode (live|async)`, timestamps

**turns**
- `id`, `hand_id`, `started_at`, `ended_at`, `duration_sec`, `skipped`

**questions**
- `id`, `hand_id`, `content`, `media_url`, `tags`, `created_at`

**answers**
- `id`, `question_id`, `scholar_id`, `content`, `media_url`, `created_at`, `is_public`

**donations**
- `id`, `user_id`, `room_id`, `question_id?`, `amount_cents`, `provider`, `tx_id`, `created_at`

---

## ⚡ Realtime Events (Socket.io / Supabase)

**Client → Server**
- `hand.raise`, `hand.lower`, `hand.keepRaisedForAsync`
- `question.submit`, `donation.create`

**Scholar → Server**
- `queue.pickRandom`, `queue.pickNext`
- `turn.start`, `turn.end`
- `answer.post`, `room.toggleAsync`

**Server → Clients**
- `queue.updated`, `turn.started`, `turn.ended`
- `cooldown.updated`, `answer.created`

---

## 🧮 Selection Logic (Pseudocode)

```ts
function pickRandomHand(hands, spokenUserIds) {
  const eligible = hands.filter(h => h.status === 'raised');
  const weighted = eligible.map(h => ({
    hand: h,
    weight: spokenUserIds.has(h.user_id) ? 1.0 : 1.5
  }));

  const sum = weighted.reduce((a, b) => a + b.weight, 0);
  let r = Math.random() * sum;
  for (const w of weighted) {
    if ((r -= w.weight) <= 0) return w.hand;
  }
  return eligible[0] ?? null;
}
