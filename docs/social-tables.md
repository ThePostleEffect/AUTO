# Social Tables & Critical Thinking Picker (AUTO)

The **Social Hall** is AUTO’s interactive community space. Instead of free-for-all chatrooms, users join **Tables** — virtual library-style hangout spots where conversation and problem-solving blend into an engaging, educational experience.

---

## 🎯 Purpose
- Create a social hub that is casual but purposeful.  
- Encourage critical thinking and teamwork in small groups.  
- Gamify learning with challenges that reward collaboration.

---

## 🪑 Tables
- Each **Table** = a small group chat (with optional voice/video).  
- Tables have a **maximum size** (6–8 people recommended).  
- Open to all, but only become “active” for challenges once **4+ users** are seated.  
- Everyday conversation is welcome, but the design nudges toward **enriching dialogue**.

---

## 🧠 Crit Think Picker
- When a Table has **4 or more participants**, the **Crit Think Picker** can be triggered.  
- Picker presents a **challenge prompt** (logic puzzle, ethical dilemma, scenario-solving exercise, etc.).  
- A **timer starts** (e.g., 10–15 minutes).  
- Users must collaborate to solve the problem before the timer expires.

---

## 🎮 Outcomes & Rewards
- **Victory** → group solves the challenge before time runs out.  
  - All participants earn **points/credits**.  
  - Credits can be spent on:  
    - cosmetic upgrades (skins, notebook covers, avatars)  
    - gifting to Scholars or peers  
    - access to bonus features.  
- **Defeat** → timer ends with no solution.  
  - No credits awarded.  
  - Users can try again with a new challenge.  

---

## ⚖️ Culture
- Social Hall = a place to hang out, laugh, and chat.  
- But the **purpose is enrichment** — every Table is designed to encourage **critical thinking, creativity, and teamwork**.  
- The Crit Think Picker transforms casual talk into **mini learning games** that reward effort and insight.

---

## 🛠 Technical Sketch
**Frontend (Next.js + Tailwind)**  
- Lobby page listing available Tables.  
- Join button → brings user into Table (chat + voice/video).  
- Crit Think Picker button (active if 4+ users).  
- Timer UI displayed once challenge starts.  

**Backend (Supabase or Node/Express)**  
- `tables` → manages active rooms.  
- `table_participants` → tracks who is at each Table.  
- `crit_think_sessions` → stores challenges, timers, outcomes.  
- `rewards` → records credits earned.  

**Realtime (Socket.io or Supabase Realtime)**  
- Sync chat, table membership, challenge prompts, and timers live across all participants.  

---

## 📊 Data Model (Simplified)

**tables**  
- `id`, `title`, `max_participants`, `created_at`

**table_participants**  
- `id`, `table_id`, `user_id`, `joined_at`

**crit_think_sessions**  
- `id`, `table_id`, `prompt_id`, `started_at`, `ends_at`, `status (active|won|lost)`

**rewards**  
- `id`, `user_id`, `session_id`, `credits`, `created_at`

---

## 🔮 Future Add-ons
- **Leaderboard** → top critical thinkers and teams.  
- **Badges** → awarded for streaks, creativity, or unique problem types.  
- **Scholar Challenges** → Scholars can inject custom prompts for bonus rewards.  

---

## ✅ Summary
The Social Hall and Crit Think Picker give AUTO its **heartbeat**. They make learning social, fun, and challenging — blending casual conversation with structured problem-solving. By rewarding collaboration, AUTO turns critical thinking into a game that is both enriching and addictive.
