const jokeSetup = document.getElementById('joke-setup');
const jokePunchline = document.getElementById('joke-punchline');
const newJokeButton = document.getElementById('new-joke-btn');

const fallbackJokes = [
  {
    type: 'two-part',
    setup: 'Why did the scarecrow become a successful entrepreneur?',
    punchline: 'Because he was outstanding in his field.'
  },
  {
    type: 'two-part',
    setup: 'What do you call a fish that practices magic?',
    punchline: 'A cod wizard.'
  },
  {
    type: 'single',
    joke: 'I used to be a baker, but I couldn’t make enough dough.'
  }
];

function renderJoke(joke) {
  if (!joke) {
    return;
  }

  if (joke.type === 'single' && joke.joke) {
    jokeSetup.textContent = joke.joke;
    jokePunchline.textContent = '';
    return;
  }

  if (joke.setup && joke.punchline) {
    jokeSetup.textContent = joke.setup;
    jokePunchline.textContent = joke.punchline;
    return;
  }

  const fallback = fallbackJokes[Math.floor(Math.random() * fallbackJokes.length)];
  renderJoke(fallback);
}

async function fetchJoke() {
  newJokeButton.disabled = true;
  jokeSetup.textContent = 'Loading a joke...';
  jokePunchline.textContent = '';

  try {
    const response = await fetch('https://official-joke-api.appspot.com/random_joke');

    if (!response.ok) {
      throw new Error('Unable to fetch joke.');
    }

    const joke = await response.json();
    renderJoke(joke);
  } catch (error) {
    console.error(error);
    const fallback = fallbackJokes[Math.floor(Math.random() * fallbackJokes.length)];
    renderJoke(fallback);
  } finally {
    newJokeButton.disabled = false;
  }
}

newJokeButton.addEventListener('click', fetchJoke);

fetchJoke();
