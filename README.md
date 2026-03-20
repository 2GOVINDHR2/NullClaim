# NullClaim — Income Protection for Delivery Partners
 
> Guidewire DEVTrails 2026 | Phase 1 Submission
 
---
 
## Why we built this
 
If you've ever ordered food on a rainy day, someone rode through that rain to bring it to you.
 
That delivery partner earns somewhere between ₹400–800 on a good day. But when a storm hits, or the AQI spikes to dangerous levels, or there's a sudden curfew in their zone — they simply can't work. No orders come in. No income. And unlike you or me, they have no sick leave, no salary backup, nothing.
 
We looked at this and thought: this is an insurance problem. A solvable one.
 
The catch is that traditional insurance doesn't work here. You can't ask a delivery partner to fill out a claim form, submit documents, and wait 2 weeks for a payout. By then the moment has passed and the damage is done. What they need is something that watches out for them automatically and puts money in their account the moment something goes wrong — without them having to do anything.
 
That's NullClaim.
 
---
 
## What it actually does
 
NullClaim is a parametric income insurance platform for food delivery partners on Swiggy and Zomato. "Parametric" means payouts are triggered automatically when a real-world parameter — like rainfall crossing 20mm/hr — is detected in the worker's zone. No claims to file. No documents to upload. No waiting.
 
Here's the flow in plain terms:
 
A delivery partner signs up, takes 3 minutes to complete onboarding, and pays a personalised weekly premium calculated by our AI based on their zone, weather exposure, and platform tenure. Our system then watches their zone around the clock. The moment rainfall, extreme heat, dangerous AQI levels, or a local disruption crosses a predefined threshold — a claim is automatically created, validated by our fraud detection layer, and the payout hits their UPI wallet. The whole thing happens in under a minute. The worker finds out via a push notification on their phone. That's it.
 
---
 
## Who we built this for
 
We focused specifically on food delivery partners — Swiggy and Zomato riders — operating in Tier-1 Indian cities. Here's a realistic picture of who they are:
 
A typical partner earns ₹500–700/day, works 6–10 hours, and has been on the platform for less than a year. They're paid weekly by the platform. They own their bike but have no financial cushion — a 3-day disruption like the monsoon hitting hard can wipe out ₹1,500–2,000 from their monthly income with no recourse.
 
These aren't edge cases. Bengaluru alone sees 60+ heavy rainfall days a year. During each one, thousands of delivery partners lose income they can never recover.
 
**Scenarios NullClaim covers:**
 
| What happened | Trigger | What the partner gets |
|---|---|---|
| Heavy rain in Koramangala | Rainfall > 20mm/hr | ₹150 for every idle hour |
| Dangerous AQI in their zone | AQI > 300 | ₹120 for every idle hour |
| Extreme heat wave | Temperature > 42°C | ₹100 for every idle hour |
| IMD issues flood red alert | Zone-level flood alert | ₹600 flat for the day |
| Local curfew or bandh | News signal detected in zone | ₹600 flat for the day |
 
We cover income loss only. No health, no accidents, no vehicle repairs — just the money they would have earned if they could work.
 
---
 
## How the weekly premium works
 
Gig workers get paid weekly, so we priced it weekly. The premium isn't a fixed number — it comes out of an ML model that looks at three things: how risky the worker's zone historically is (flood-prone areas cost more), how much weather exposure they face based on seasonal forecasts, and how long they've been on the platform (longer tenure gets a small loyalty discount).
 
The range is ₹49–₹149/week depending on these factors. Maximum coverage is ₹3,500/week — roughly 5 working days at average earnings.
 
The premium gets auto-deducted from their platform wallet every Monday. No bank account needed, no UPI setup headache.
 
---
 
## The triggers that fire claims
 
These are the exact thresholds our system monitors:
 
| Trigger | Threshold | Payout |
|---|---|---|
| Rainfall | > 20mm/hr in worker's zone | ₹150/hr idle |
| Extreme heat | Temperature > 42°C | ₹100/hr idle |
| Air quality | AQI > 300 (Hazardous) | ₹120/hr idle |
| Flood alert | IMD red alert in zone | ₹600/day flat |
| Curfew or bandh | Detected via news signals | ₹600/day flat |
| Platform outage | Order volume drops > 70% in zone | ₹200/hr idle |
 
The system polls weather and AQI APIs every 15 minutes. When a threshold is crossed, it doesn't just trigger one claim — it identifies every active policy holder in that zone and fires claims for all of them simultaneously.
 
---
 
## How the AI fits in
 
We use three models, each doing a specific job:
 
**Premium calculator (XGBoost)** — takes in the worker's zone, their activity history, weather exposure for their area, and platform tenure, and spits out a personalised weekly premium. This is what makes pricing fair and zone-specific rather than one-size-fits-all.
 
**Risk profiler (Random Forest)** — runs at onboarding to bucket the worker into Low, Medium, or High risk. This feeds into the premium calculator and also helps the system decide how closely to monitor that worker's zone.
 
**Fraud detector (Isolation Forest)** — this is our differentiator. Every claim gets run through this before payout. It checks whether the worker's GPS was actually idle during the disruption window, whether other workers in the same zone also got claims (genuine disruptions affect everyone, fake ones don't), and whether the claim pattern matches anything suspicious in their history. It flags anomalies automatically.
 
Since we don't have historical claim data for a product that doesn't exist yet, we trained the models on synthetic data — 5,000 worker profiles generated using domain rules, anchored with real historical weather data from Open-Meteo for Bengaluru and Mumbai over the past 12 months. This is standard practice in actuarial modeling for new insurance products.
 
---
 
## What we're building on
 
| Layer | What we're using |
|---|---|
| Frontend | React Native + Expo |
| Backend | FastAPI (Python) |
| ML | scikit-learn, XGBoost |
| Database | PostgreSQL |
| Weather | OpenWeatherMap + Open-Meteo |
| AQI | OpenAQ |
| News signals | GNews API |
| Payments | Razorpay test mode |
| Scheduler | APScheduler (runs the disruption monitor) |
| Hosting | Render (backend), Expo Go / TestFlight (mobile app) |
 
We chose a mobile app because delivery partners are always on their phones between orders. Onboarding, checking policy status, and receiving payout notifications all happen naturally on mobile. The admin dashboard is accessible via a responsive web view on the same codebase.
 
---
 
## Project structure
 
```
NullClaim/
├── mobile/
│   └── src/
│       ├── screens/      — Onboarding, Dashboard, Claims
│       ├── components/   — Shared UI components
│       └── navigation/   — React Navigation stack
├── backend/
│   ├── routers/          — auth, policy, claims, payout
│   ├── ml/               — premium model, fraud model, data generation
│   └── scheduler/        — disruption monitor (APScheduler)
├── data/
│   └── synthetic_training_data.csv
└── README.md
```
 
---
 
## Team
 
- Govindh R
- Abhinav G Nair
- Meenakshi
- Jaith Jayan
 
---
 
## Demo video
 
[Watch our Phase 1 demo video](https://youtu.be/yNP4P75QdVM)
 
---
