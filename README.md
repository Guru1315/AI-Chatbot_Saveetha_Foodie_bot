🚀 **Live app:** https://ai-chatbot-pvj39rjn32zxtyxtlqrehq.streamlit.app/

# 🍽️ Saveetha Foodie Bot

A campus-focused food chatbot that helps students and staff of Saveetha University discover where to eat nearby, quickly and conversationally.

## 📌 Overview

Finding good, affordable food around campus usually means asking friends or scrolling through scattered Google Maps reviews. **Saveetha Foodie Bot** solves this by letting users simply ask for what they want, such as *"Where can I get biriyani under ₹150?"*, and get relevant suggestions instantly.

The bot is a chat interface on top of Google's **Gemini** model. It is given one text file of student-recommended food spots and facts about the university, and it is told to answer only from that file. If a question is not covered, it is instructed to say it does not have that information.

## ✨ Features

- 💬 Conversational chat interface for food queries, with the conversation remembered while the page stays open
- 🍛 Recommendations by dish: biriyani, egg rice, meals and fried rice
- 💰 Prices for the biriyani and egg rice spots, so you can ask for budget options
- 📍 Landmark-based locations (for example "near SCAD building" or "under Palar Hostel")
- 🏫 Answers about Saveetha University itself: history, accreditation, rankings, placements and programs
- 💡 Four one-tap starter questions on the empty chat screen
- ⚡ Simple to run: paste a Gemini API key in the sidebar and start chatting

## 🧠 How it works

```
saveetha_university_infos.txt  ──►  system prompt  ──►  Gemini chat  ◄──  your question
        (knowledge base)           ("answer only from this")                  │
                                                                              ▼
                                                              answer shown in the Streamlit chat
```

1. `app.py` reads `saveetha_university_infos.txt` and puts the whole file into the model's system instruction.
2. Each question you type is sent to a Gemini chat session (`gemini-3.6-flash`), which replies using that text.
3. The messages are kept in Streamlit's session state, so they are cleared when you refresh the page.

There is no search step: the entire file is sent every time, which works because the file is small.

## 🛠️ Tech Stack

- **Language:** Python (the dev container uses 3.11)
- **Chatbot logic:** LLM (Google Gemini through the `google-genai` library), grounded in a plain-text knowledge base through the system prompt
- **Data:** `saveetha_university_infos.txt`, a hand-written text file (not a database)
- **Interface:** Streamlit web app
- **Hosting:** Streamlit Community Cloud

## 🚀 Getting Started

You need a Google Gemini API key. You can create one at https://aistudio.google.com/apikey.

```bash
# Clone the repository
git clone https://github.com/Guru1315/AI-Chatbot_Saveetha_Foodie_bot.git
cd AI-Chatbot_Saveetha_Foodie_bot

# Install dependencies
pip install -r requirements.txt

# Run the app
streamlit run app.py
```

Then open http://localhost:8501 and paste your key into the **Google Gemini API Key** box in the sidebar.

Instead of typing the key each time, you can set the `GOOGLE_API_KEY` environment variable before starting the app, and it will be used when nothing is entered in the sidebar. Never commit your key to the repository.

## 💡 Example Queries

- "Suggest a good biriyani place near campus"
- "Which biriyani is the cheapest?"
- "Where can I get egg rice?"
- "Where can I eat near Equatorial Garden?"
- "Tell me about Saveetha University"

## 🍽️ What the food guide covers

| Dish | Spots listed | Price given |
|---|---|---|
| Biriyani | Creamy Spoon (₹130), Ram Cafe (₹110), Creamy and Crunchy (₹120) | Yes |
| Egg rice | Sai's Kitchen (₹60) | Yes |
| Meals | Sree Caters, SS Caters | No |
| Fried rice | Sree Caters, RR Restaurant, Anukraha | No |

Because the meals and fried rice spots have no prices in the file, a question like *"Cheap meals under ₹100"* cannot be answered from the data, and the bot should say it does not have that information.

## 📂 Project Structure

```
app.py                          The Streamlit app: page, sidebar, Gemini chat
saveetha_university_infos.txt   Knowledge base: university facts and the campus food guide
saveetha_campus_foodie_bot.txt  Early command-line version of the bot (a script saved as .txt, not used by the app)
requirements.txt                Python dependencies (streamlit, google-genai)
.devcontainer/devcontainer.json Python 3.11 dev container / Codespaces setup, serves on port 8501
README.md                       This file
```

### Updating the food guide

Edit section 5, **Campus Food Guide**, in `saveetha_university_infos.txt`. The bot picks up the new text the next time the app starts. Note that the **Popular Spots** card in the sidebar is written into `app.py`, so it does not change when you edit the text file.

## ⚠️ Limitations

- The data is student input. Prices and locations may be out of date, and the file itself says so.
- University rankings and placement figures change every year, so check the official SIMATS website for current numbers.
- The model is told to stay inside the knowledge base, but it is an AI model and can still occasionally answer from general knowledge or get a detail wrong.
- Locations are landmarks, not distances or map directions.
- There is no memory between visits, and no user accounts or saved history.
- Library versions are not pinned in `requirements.txt`.

## 🔮 Future Improvements

- Real-time ratings and user reviews
- Opening hours and live availability
- Menu and price updates by the community
- Multi-language support (Tamil/English)

## 🎓 About

Built as a student project at **Saveetha University (SIMATS Engineering)** to make campus food discovery easier for everyone.
