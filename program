import streamlit as st
import random
import time

# --- APP CONFIGURATION ---
st.set_page_config(page_title="Fun Fishing Frenzy", page_icon="🎣")

# --- INITIALIZE SESSION STATE ---
# This acts as the "memory" of the app so it doesn't reset every click
if "challenge" not in st.session_state:
    st.session_state.challenge = None
if "trophies" not in st.session_state:
    st.session_state.trophies = []
if "show_success" not in st.session_state:
    st.session_state.show_success = False

# --- DATA ---
fish_data = {
    "Lake": ["Largemouth Bass", "Bluegill", "Crappie", "Walleye", "Catfish"],
    "Pond": ["Bass", "Sunfish", "Bullhead", "Carp"],
    "River": ["Smallmouth Bass", "Trout", "Catfish", "Pike"],
    "Anywhere": ["Bluegill", "Catfish", "Bass"],
    "Bay": ["Redfish", "Speckled Trout", "Flounder", "Snook", "Striped Bass"],
    "Open Ocean": ["Mahi Mahi", "Tuna", "Grouper", "Snapper", "Kingfish"]
}
baits = ["Nightcrawlers", "Spinnerbaits", "Plastic Worms", "Crankbaits", "Live Shrimp", "Spoons", "Corn"]
templates = [
    "Catch a {species} using {bait}",
    "Catch a {species} on your very first cast",
    "Catch 3 different fish within 30 minutes",
    "Catch a {species} larger than {size} inches",
    "Catch any fish using a topwater lure",
    "Land a fish without using a net"
]

def get_new_challenge(loc):
    template = random.choice(templates)
    species = random.choice(fish_data[loc])
    bait = random.choice(baits)
    size = random.randint(12, 28)
    return template.format(species=species, bait=bait, size=size)

# --- SIDEBAR: SETTINGS ---
st.sidebar.title("🎣 Fishing Settings")
mode = st.sidebar.radio("Environment", ["Freshwater", "Saltwater"])

if mode == "Freshwater":
    location = st.sidebar.selectbox("Location", ["Lake", "Pond", "River", "Anywhere"])
else:
    location = st.sidebar.selectbox("Location", ["Bay", "Open Ocean"])

if st.sidebar.button("Reset Game"):
    st.session_state.challenge = None
    st.session_state.trophies = []
    st.rerun()

# --- MAIN APP INTERFACE ---
st.title("Fun Fishing Frenzy")
st.write(f"Currently fishing in: **{location}**")

# Celebration Logic
if st.session_state.show_success:
    st.success("CHALLENGE COMPLETE!", icon="✅")
    st.balloons()
    time.sleep(1) # Brief pause for effect
    st.session_state.show_success = False

# Challenge Display
if st.session_state.challenge is None:
    if st.button("Generate First Challenge"):
        st.session_state.challenge = get_new_challenge(location)
        st.rerun()
else:
    st.info(f"### {st.session_state.challenge}")
    
    col1, col2, col3 = st.columns(3)
    
    with col1:
        if st.button("✅ I Did It!"):
            st.session_state.trophies.append(f"{location}: {st.session_state.challenge}")
            st.session_state.challenge = get_new_challenge(location)
            st.session_state.show_success = True
            st.rerun()
            
    with col2:
        if st.button("⏭️ Skip"):
            st.session_state.challenge = get_new_challenge(location)
            st.rerun()

# --- TROPHY ROOM ---
st.divider()
st.subheader("🏆 Your Trophy Room")
if st.session_state.trophies:
    for t in reversed(st.session_state.trophies):
        st.write(f"- {t}")
else:
    st.write("No trophies yet. Get out there and fish!")
