<script setup>
import { animate, spring} from "motion";
import { ref, onMounted, onUnmounted } from 'vue';
import GithubButton from '~/components/GithubButton.vue';
import LinkedInButton from '~/components/LinkedInButton.vue';
import Navbar from '~/components/Navbar.vue';
import Hero from '~/components/Hero.vue';
import About from '~/components/About.vue';

// Landing content animations
// TODO

// Section divider animations
const topTitle = ref(null);
const topDescription = ref(null);
const bottomTitle = ref(null);
const bottomDescription = ref(null);

let hasScrolled = false; // Track if user has scrolled down initially

const handleScroll = () => {
  if (!hasScrolled && window.scrollY > 50) { // Detect first downward scroll
    hasScrolled = true;

    // Animate top column elements
    animate(topTitle.value, { x: [150, 0], opacity: [0, 1] }, { type: spring, bounce: 0.3, duration: 0.8 });
    animate(topDescription.value, { x: [150, 0], opacity: [0, 0.5] }, { type: spring, bounce: 0.4, duration: 0.8 });

    window.removeEventListener("scroll", handleScroll); // Remove event listener after animation
  }
};

const observerCallback = (entries, observer) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      if (entry.target === bottomTitle.value) {
        animate(bottomTitle.value, { x: [-150, 0], opacity: [0, 1] }, { type: spring, bounce: 0.4, duration: 0.8 });
        animate(bottomDescription.value, { x: [-150, 0], opacity: [0, 0.5] }, { type: spring, bounce: 0.3, duration: 0.8 });
        observer.unobserve(entry.target); // Stop observing after animation runs once
      }
    }
  });
};

onMounted(() => {
  // Track the first scroll event
  window.addEventListener("scroll", handleScroll);

  // Use Intersection Observer for bottom elements
  const observer = new IntersectionObserver(observerCallback, { threshold: 0.2 });
  if (bottomTitle.value) observer.observe(bottomTitle.value);
  if (bottomDescription.value) observer.observe(bottomDescription.value);
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
});

</script>

<template>
    <div id="app">
        <!-- Background -->
        <div class="background"></div>

        <!-- Container for constrained content -->
        <div class="container">
            <!-- Navigation Bar -->
            <Navbar />

            <!-- Landing Section -->
            <div class="landing-content">
                <h1 class="landing-title animate-text">TragSync, a way to monitor <br> your pellet smoker on the web</h1>
                <p class="landing-description animate-text">
                    Our favorite pellet smoker company didn't make an official web solution to let us
                    <br> monitor our grills, so I made one myself. Free & open source.
                </p>
                <div class="flex items-center justify-center animate-text">
                    <NuxtLink to="/grill" class="get-started-button">
                        <span class="button-text">Get Started</span>
                    </NuxtLink>
                    <GithubButton width="45" height="45"/>
                </div>
            </div>
            
            <!-- Section Divider -->
            <section class="section-with-background-white">
                <div class="flex-col">
                    <!-- Top Column -->
                    <div class="flex-col justify-end items-start text-right overflow-hidden">
                        <p class="landing-section-description mt-[3.7rem]" ref="topDescription">
                            View your pellet smoker in real-time from your phone,<br>
                            tablet, or computer. Track your cook from your web browser.
                        </p>
                        <h2 class="landing-section-title mt-[0.75rem]" ref="topTitle">Sync Your Grill, Master The Flame</h2>
                    </div>

                    <!-- Middle Demo Image -->
                    <!-- WITH SHADOW -->
                    <img src="../assets/traeger_web-ui-app.png" 
                         alt="grill app demo" 
                         class="w-full rounded-[20px] mt-[1.75rem] mb-[1.75rem]"
                    >
                    <!-- NO SHADOW -->
                    <!-- <img src="../assets/traeger_web-ui-app.png" 
                         alt="grill app demo" 
                         class="w-full rounded-[20px] mt-[1.75rem] mb-[1.75rem] shadow-[8px_8px_30px_rgb(0,0,0,0.4)]"
                    > -->
                
                    <!-- Bottom Column -->
                    <div class="flex-col justify-end items-end text-left overflow-hidden">
                        <h2 class="landing-section-title mb-[0.75rem]" ref="bottomTitle">Check Your Smoker Anytime, Anywhere</h2>
                        <p class="landing-section-description mb-[3.7rem]" ref="bottomDescription">
                            With this custom web interface you can grill like a pro, even<br>
                            when you’re away from the smoker.
                        </p>
                    </div>
                </div>
            </section>

            <!-- About Section -->
            <section class="section-about">
                <About />
            </section>
        </div>

        

        
        <!-- Footer -->
        <footer class="footer">
            <p class="footer-text">TragSync is a free and open source web application created and maintained by Wesly Barayuga</p>
            <GithubButton class="ml-[2px]" width="28" height="28"/>
            <LinkedInButton class="pt-[2px]" width="35px" height="35px" link="https://www.linkedin.com/in/wesly-barayuga/"/>
        </footer>
    </div>
</template>

<style scoped>
/* Global Styles */
body,
html {
    margin: 0;
    padding: 0;
    font-family: 'Inter', sans-serif;
    height: 100%;
    overflow-x: hidden;
}

#app {
    display: flex;
    flex-direction: column;
    min-height: 250vh; 
    position: relative;
    overflow-x: hidden;
}

/* Background */
.background {
    position: absolute;
    width: 100%;
    height: 100%; /* Ensure it covers the full height */
    /* background: linear-gradient(90deg, #535353 0.01%, rgba(196, 113, 62, 0.99) 65.5%, #ffffff 100%); */
    background: linear-gradient(90deg, #000000 0.01%, rgba(196, 113, 62, 0.99) 65.5%, #ffffff 100%);
    z-index: -1;
    top: 0;
    left: 0;
}

/* Container to constrain content */
.container {
    width: 80%; /* Matches .landing-title */
    max-width: 1200px; /* Ensures a consistent max width */
    margin: 0 auto; /* Centers the content */
}

/* Landing Section */
.landing-content {
    width: 100%;
    max-width: 1200px;
    margin: 15rem auto;
    /* padding: 50px 0; */
    /* padding-bottom: 20px; */
}

.landing-content .flex {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 12px; 
    margin-top: 1.5rem; 
}

.landing-title {
    font-size: clamp(2rem, 5vw, 76px); /* Scales between 2.5rem and 5rem */
    line-height: 1.4;
    font-weight: 900;
    text-align: center;
    color: #000000;
    /* opacity: 0.9; */
    width: 100%;
    overflow: hidden;
}

.landing-description {
    font-size: clamp(0.5rem, 2.5vw, 1.5rem); /* Adjusts between 1rem and 1.5rem */
    font-weight: 400;
    text-align: center;
    color: #00000094;
    opacity: 0.9;
    margin-top: 1rem;
    width: 100%;
}

/* Get Started Button */
.get-started-button {
    box-sizing: border-box;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    padding: 12px;
    gap: 8px;
    position: relative;
    width: 134px;
    height: 47px;
    background: rgba(44, 44, 44, 0.45);
    border-radius: 20px;
    backdrop-filter: blur(18.5px);
    -webkit-backdrop-filter: blur(4px);
    margin: 0;
    transition: background-color 0.1s;
    text-align: center;
    cursor: pointer;
}

.get-started-button:hover {
    background: rgba(26, 26, 26, 0.50);
}

.button-text {
    font-size: clamp(0.875rem, 1.5vw, 1rem);
    color: #f5f5f5;
    text-decoration: none;
}

/* Container for the section with the rotated background */
.section-with-background-white {
    position: relative;
    /* overflow: hidden; /* Hide any overflow caused by the rotated background */
    margin-top: 7rem;
    padding-top: 1rem;
    padding-bottom: 1rem;
}

/* Rotated background using a pseudo-element */
.section-with-background-white::before {
    content: '';
    position: absolute;
    width: 150vw; /* Wider than the viewport to account for rotation */
    height: 100%; /* Full height of the container */
    left: 50%;
    top: 0;
    transform: translateX(-50%) rotate(-8deg); /* Center and rotate */
    z-index: -1; /* Place it behind the content */
    /* background: rgba(255, 255, 255, 0.8);  */
    background: rgba(255, 255, 255, 0.4); 
}

.landing-section-title {
    font-size: clamp(1.3rem, 3vw, 40px); /* Scales between 2.5rem and 5rem */
    font-weight: 900;
    /* text-align: center; */
    color: #ffffff;
    /* opacity: 0.9; */
    /* width: 100%; */
    background: rgba(0, 0, 0, 0.4); 
    border-radius: 30px;
    padding: 0.75rem;
    display: inline-block; 
    /* box-shadow: 0px 0px 20px 0px rgba(0, 0, 0, 0.15); */
    backdrop-filter: blur(18.5px);
    -webkit-backdrop-filter: blur(4px);
    border: 1px solid rgba(255, 255, 255, 0.15);
}

.landing-section-description {
    font-size: clamp(0.75rem, 1.5vw, 20px); /* Scales between 2.5rem and 5rem */
    font-weight: 700;
    /* text-align: center; */
    color: #ffffff;
    /* opacity: 0.75; */
    /* margin-top: 10rem; */
    /* width: 100%; */
    line-height: 2rem;
    background: rgba(0, 0, 0, 0.5); 
    border-radius: 30px;
    padding: 0.75rem;
    display: inline-block; 
    /* box-shadow: 0px 0px 20px 0px rgba(0, 0, 0, 0.15); */
    backdrop-filter: blur(18.5px);
    -webkit-backdrop-filter: blur(4px);
    border: 1px solid rgba(255, 255, 255, 0.15);
}


/* Footer */
.footer {
    display: flex;
    flex-direction: row; 
    justify-content: center;
    align-items: center;
    gap: 0.5rem;
    width: 100%;
    padding: 0.5rem;
    background: rgba(0, 0, 0, 0.15);
    opacity: 0.75;
    text-align: center;
    margin-top: auto;
}

.footer-text {
    font-size: 1rem;
    color: #f5f5f5;
    opacity: 0.6;
}

/* TABLET VIEW */
@media (max-width: 768px) {
    .landing-title {
        font-size: clamp(1rem, 5vw, 40px);
    }

    .landing-description {
        font-size: clamp(0.5rem, 2vw, 20px); 
    }

    .section-with-background-white {
        /* padding: 3rem 0;  */
    }

    .landing-section-description {
        line-height: 1.5rem;
    }

    /* Handle footer text resizing and maybe buttons tool? */
}

@media (max-width: 425px) {
    .section-with-background-white {
        /* padding: 4rem 0;  */
    }

    .landing-description {
        font-size: clamp(0.4rem, 2vw, 20px); 
    }
}


/* Animation for text */
.animate-text {
    opacity: 0; /* Start with invisible text */
    transform: translateY(12px); /* Start slightly below */
    animation: fadeInUp 0.5s ease-out forwards; /* Apply animation */
}

/* Delay animations for each element */
.landing-title.animate-text {
    animation-delay: 0.1s; /* Delay for the title */
}

.landing-description.animate-text {
    animation-delay: 0.2s; /* Delay for the description */
}

.get-started-button.animate-text {
    animation-delay: 0.3s; /* Delay for the button */
}

/* Keyframes for the fadeInUp animation */
@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

/* Ensure animation starts with invisible text */
.landing-section-title,
.landing-section-description {
    opacity: 0;
}

.section-about {
    margin-top: 20rem;
}
</style>
