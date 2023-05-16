Code snippet
# README.md
## JavaScript code


```jsx
'use strict';

///////////////////////////////////////
// Modal window

const modal = document.querySelector('.modal');
const overlay = document.querySelector('.overlay');
const btnCloseModal = document.querySelector('.btn--close-modal');
const btnsOpenModal = document.querySelectorAll('.btn--show-modal');
const nav = document.querySelector('.nav');

const openModal = function (e) {
  e.preventDefault();

  modal.classList.remove('hidden');
  overlay.classList.remove('hidden');
};

const closeModal = function () {
  modal.classList.add('hidden');
  overlay.classList.add('hidden');
};

// for (let i = 0; i < btnsOpenModal.length; i++)
//   btnsOpenModal[i].addEventListener('click', openModal);

btnsOpenModal.forEach(btn => btn.addEventListener('click', openModal));

btnCloseModal.addEventListener('click', closeModal);
overlay.addEventListener('click', closeModal);

document.addEventListener('keydown', function (e) {
  if (e.key === 'Escape' && !modal.classList.contains('hidden')) {
    closeModal();
  }
});

// Creating and inserting elements
const message = document.createElement('div');
message.classList.add('cookie-message');

const header = document.querySelector('header');

message.innerHTML =
  'We use cookies for improved functionality and analytics. <button class="btn btn--close-cookie">Got it!</button>';

header.append(message);

//Delete elements
document
  .querySelector('.btn--close-cookie')
  .addEventListener('click', function () {
    message.remove();
  });

// Styles
message.style.backgroundColor = '#37383d';
message.style.width = '105%';

message.style.height =
  Number.parseFloat(getComputedStyle(message).height) + 40 + 'px';

const btnScrollTo = document.querySelector('.btn--scroll-to');
const section1 = document.querySelector('#section--1');
const section2 = document.querySelector('#section--2');
const section3 = document.querySelector('#section--3');
const section4 = document.querySelector('#section--4');

btnScrollTo.addEventListener('click', function (e) {
  const s1coords = section1.getBoundingClientRect();

  console.log(
    'height/width',
    document.documentElement.clientHeight,
    document.documentElement.clientWidth
  );

  //Scrolling
  window.scrollTo(
    s1coords.left + window.pageXOffset,
    s1coords.top + window.pageYOffset
  );

  //use of smooth scrolling
  window.scrollTo({
    left: s1coords.left + window.pageXOffset,
    top: s1coords.top + window.pageYOffset,
    behavior: 'smooth',
  });

  //more advanced way
  section1.scrollIntoView({ behavior: 'smooth' });
});
const h1 = document.querySelector('h1');

// rgb(255, 255, 0)
const randomInt = (min, max) =>
  Math.floor(Math.random() * (max - min + 1) + min);

const randomColor = () =>
  `rgb(${randomInt(0, 255)},${randomInt(0, 255)},${randomInt(0, 255)})`;

// use of event propagation
document.querySelector('.nav__link').addEventListener('click', function (e) {
  this.style.backgroundColor = randomColor();
});

document.querySelector('.nav__links').addEventListener('click', function (e) {
  this.style.backgroundColor = randomColor();
});

document.querySelector('.nav').addEventListener('click', function (e) {
  this.style.backgroundColor = randomColor();
});

// use of tabbed component
const tabs = document.querySelectorAll('.operations__tab');
const tabsContainer = document.querySelector('.operations__tab-container');
const tabsContent = document.querySelectorAll('.operations__content');

tabsContainer.addEventListener('click', function (e) {
  const clicked = e.target.closest('.operations__tab');

  //Guard clause
  if (!clicked) return;

  // Activate content area
  const btn_number = clicked.dataset.tab;

  tabsContent.forEach(ct => {
    ct.classList.remove('operations__content--active');
  });

  tabsContent[btn_number - 1].classList.add('operations__content--active');

  //Active tab
  tabs.forEach(t => {
    t.classList.remove('operations__tab--active');
    clicked.classList.add('operations__tab--active');
  });
});

//Menu Fade animation | use of event delegation | use of bind
const event_delegator = function (e, opacity) {
  if (e.target.classList.contains('nav__link')) {
    const link = e.target;
    const siblings = link.closest('.nav').querySelectorAll('.nav__link');
    const logo = link.closest('.nav').querySelector('svg');

    siblings.forEach(e => {
      if (e !== link) e.style.opacity = this;
      logo.style.opacity = this;
    });
  }
};

nav.addEventListener('mouseover', event_delegator.bind(0.5));
nav.addEventListener('mouseout', event_delegator.bind(1));

const section1_coords = section1.getBoundingClientRect();
const navHeight = nav.getBoundingClientRect().height;

const stickyNav = function (entries, observer) {
  const [entry] = entries;

  if (!entry.isIntersecting) {
    nav.classList.add('sticky');
  } else {
    nav.classList.remove('sticky');
  }
};

const observer = new IntersectionObserver(stickyNav, {
  root: null,
  threshold: 0,
  rootMargin: `-${navHeight}px`, // only accept the arguement in pixels only
});
observer.observe(header);

//////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////
//Revealing elements on scroll
const observerFunction = function (entries, observer) {
  const [entry] = entries;
  if (!entry.isIntersecting) return;

  entry.target.classList.remove('section--hidden');
  observer2.unobserve(entry.target);
};

const observer2 = new IntersectionObserver(observerFunction, {
  root: null,
  threshold: 0.1,
});

document.querySelectorAll('.section').forEach(s => {
  s.classList.add('section--hidden');
  observer2.observe(s);
});
//////////////////////////////////////////////////////////////
//////////////////////////////////////////////////////////////

// use of lazy loading
const images = document.querySelectorAll('img[data-src]');

const imageobserver = function (entries, observer) {
  const [entry] = entries;

  if (!entry.isIntersecting) return;
  entry.target.src = entry.target.dataset.src;

  entry.target.addEventListener('load', () => {
    entry.target.classList.remove('lazy-img');
  });
};

const observer3 = new IntersectionObserver(imageobserver, {
  root: null,
  threshold: 0,
  rootMargin: '400px',
});
images.forEach(img => {
  observer3.observe(img);
});
/////////////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////////

// use of slider component
// use of dots component
// use of arrow component
const slider = function () {
  const sliderSection = document.querySelector('#section--3');
  const slides = document.querySelectorAll('.slide');
  const btnRight = document.querySelector('.slider__btn--right');
  const btnLeft = document.querySelector('.slider__btn--left');
  const dotContainer = document.querySelector('.dots');

  let maxSlide = slides.length;
  let crSlide = 0;

  // Functions
  const slideMover = function (crSlide) {
    slides.forEach((slide, index) => {
      slide.style.transform = `translateX(${100 * (index - crSlide)}%)`;
    });
  };
  const activateDots = function (slide) {
    document
      .querySelectorAll('.dots__dot')
      .forEach(dot => dot.classList.remove('dots__dot--active'));

    document
      .querySelector(`.dots__dot[data-slide ='${slide}']`)
      .classList.add('dots__dot--active');
  };
  const createDots = function () {
    slides.forEach((_, i) => {
      dotContainer.insertAdjacentHTML(
        'beforeend',
        `<button class='dots__dot' data-slide='${i}'></button>`
      );
    });
  };
  const nextSlide = function () {
    if (crSlide === maxSlide - 1) crSlide = 0;
    else crSlide++;
    slideMover(crSlide);
    activateDots(crSlide);
  };
  const prevSlide = function () {
    if (crSlide == 0) crSlide = maxSlide - 1;
    else crSlide--;
    slideMover(crSlide);
    activateDots(crSlide);
  };

  const init = function () {
    slideMover(0);
    createDots();
    activateDots(0);
  };

  const slider_mover_with_arrow_click = function (e) {
    if (e.key === 'ArrowRight') nextSlide();
    if (e.key === 'ArrowLeft') prevSlide();
  };

  const keydownEvent = function (entries, observer) {
    const [entry] = entries;
    entry.isIntersecting
      ? document.addEventListener('keydown', slider_mover_with_arrow_click)
      : document.removeEventListener('keydown', slider_mover_with_arrow_click);
  };

  init();

  //Event Listeners
  btnRight.addEventListener('click', nextSlide);
  btnLeft.addEventListener('click', prevSlide);

  dotContainer.addEventListener('click', function (e) {
    if (e.target.classList.contains('dots__dot')) {
      const { slide } = e.target.dataset;

      slideMover(slide);
      activateDots(slide);
    }
  });

  //on pressing the Arrow keys
  const observer4 = new IntersectionObserver(keydownEvent, {
    root: null,
    threshold: 0,
  });
  observer4.observe(sliderSection);
};
slider();
```

# Mapty

```jsx
'use strict'

class Workout {
  type;
  date = new Date();
  id = Date.now() + ''.slice(-10);

  constructor(coords, distance, duration) {
    this.coords = coords; // [lat, lng]
    this.distance = distance; // in km
    this.duration = duration; // in min
  }

  _setDescription() {
    // prettier-ignore
    const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];

    this.description = `${
      this.type === 'running' ? '🏃‍♂️' : '🚴‍♀️'
    } ${this.type[0].toUpperCase()}${this.type.slice(1)} on ${
      months[this.date.getMonth()]
    } ${this.date.getDate()}`;
  }
}

class Running extends Workout {
  type = 'running';
  constructor(coords, distance, duration, cadence) {
    super(coords, distance, duration);
    this.cadence = cadence;
    this.calcPace();
    this._setDescription();
  }
  calcPace() {
    //min/km
    this.pace = this.duration / this.distance;
  }
}

class Cycling extends Workout {
  type = 'cycling';
  constructor(coords, distance, duration, elevationGain) {
    super(coords, distance, duration);
    this.elevationGain = elevationGain;
    this.calcSpeed();
    this._setDescription();
  }
  calcSpeed() {
    //km/h
    this.speed = this.distance / (this.duration / 60);
    return this.speed;
  }
}

/////////////////////////////////////////////////////////////////////////////////////////////
// APLLICATION ARCHITECTURE
const form = document.querySelector('.form');
const containerWorkouts = document.querySelector('.workouts');
const inputType = document.querySelector('.form__input--type');
const inputDistance = document.querySelector('.form__input--distance');
const inputDuration = document.querySelector('.form__input--duration');
const inputCadence = document.querySelector('.form__input--cadence');
const inputElevation = document.querySelector('.form__input--elevation');

class App {
  // [rivate properties  | private fields
  #map;
  #mapZoomLevel = 13;
  #mapEvent;
  #workout = [];
  #data;

  constructor() {
    this._getLocalStorage();
    this._getPosition();
    inputType.addEventListener('change', this._toggleElevationField);

    /*
    The solution of this error is to use bind method to set the this keyword

    script3.js:155 Uncaught TypeError: Cannot read private member #mapEvent from an object whose class did not declare it
    at HTMLFormElement._newWorkout (script3.js:155:31)
    */
    form.addEventListener('submit', this._newWorkout.bind(this));
    containerWorkouts.addEventListener('click', this._moveMap.bind(this));
  }

  _getPosition() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(this._loadMap.bind(this), e =>
        console.error(e.message)
      );
    }
  }

  _loadMap(position) {
    // destructuring object
    const { latitude, longitude } = position.coords;
    const coords = [latitude, longitude];

    this.#map = L.map('map').setView(coords, 13);

    L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution:
        '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    }).addTo(this.#map);

    // Setting the storage markers
    this._getMarker(this.#data);

    // Handling clicks on map
    this.#map.on('click', this._showForm.bind(this));
  }

  _showForm(mapE) {
    this.#mapEvent = mapE;
    form.classList.remove('hidden');
    inputDistance.focus();
  }
  _hideform() {
    // Clear input fields
    inputCadence.value =
      inputDistance.value =
      inputDuration.value =
      inputElevation.value =
        '';
    form.style.display = 'none';
    form.classList.add('hidden');
    setTimeout(() => ((form.style.display = 'grid'), 1000));
  }
  _moveMap(e) {
    const workoutEl = e.target.closest('.workout');

    if (!workoutEl) {
      return;
    }

    const workout = this.#workout.find(
      obj => obj.id === workoutEl.dataset.id
    ).coords; // getting the coords of current object

    this.#map.setView(workout, this.#mapZoomLevel, {
      animate: true,
      pan: { duration: 1 },
    }); // ([lat, lng], zoomValue , {options})
  }

  _toggleElevationField() {
    inputElevation.closest('.form__row').classList.toggle('form__row--hidden');
    inputCadence.closest('.form__row').classList.toggle('form__row--hidden');
  }

  _newWorkout(e) {
    e.preventDefault();
    let workout;

    // helper functions
    const validInputs = (...inputs) => inputs.every(e => Number.isFinite(e));
    const allPositive = (...inputs) => inputs.every(e => e > 0);

    const { lat, lng } = this.#mapEvent.latlng;

    // Get common data from form
    const type = inputType.value;
    const distance = +inputDistance.value;
    const duration = +inputDuration.value;

    // If activity running, create running object
    if (type === 'running') {
      const cadence = +inputCadence.value;

      // Check if data is valid
      if (
        !validInputs(distance, duration, cadence) ||
        !allPositive(distance, duration, cadence)
      ) {
        return alert('Inputs have to be positive numbers');
      }
      // calling class inside a class
      workout = new Running([lat, lng], distance, duration, cadence);
    }

    // If activity cycling, create cycling object
    if (type === 'cycling') {
      const elevation = +inputElevation.value;

      // Check if data is valid
      if (
        !validInputs(distance, duration, elevation) ||
        !allPositive(distance, duration)
      ) {
        return alert('Inputs have to be positive numbers');
      }
      // calling class inside a class
      workout = new Cycling([lat, lng], distance, duration, elevation);
    }

    // Add new object to workout array
    console.log(workout);
    this.#workout.push(workout);

    // Render workout on map as marker
    this._renderWorkoutMarker(workout);

    // Render workout on list
    this._renderWorkout(workout);

    // adding the delete event Listner
    document
      .querySelector('.delete')
      .addEventListener('click', this._deleteWorkout.bind(this));

    // Hiding the form
    this._hideform();

    // Set all the items to the local storage
    this._setLocalStorage();
  }

  _renderWorkout(workout) {
    let html = `
    <li class="workout workout--${workout.type}" data-id="${workout.id}">
    <div class="top__elements">
          <h2 class="workout__title">${workout.description.substring(4)}</h2>
          <div class="all_svg">
             <svg xmlns="http://www.w3.org/2000/svg" class="edit" width="22" height="22" fill="#aaa" viewBox="0 0 256 256"><path d="M230.14,70.54,185.46,25.85a20,20,0,0,0-28.29,0L33.86,149.17A19.85,19.85,0,0,0,28,163.31V208a20,20,0,0,0,20,20H92.69a19.86,19.86,0,0,0,14.14-5.86L230.14,98.82a20,20,0,0,0,0-28.28ZM91,204H52V165l84-84,39,39ZM192,103,153,64l18.34-18.34,39,39Z"></path></svg>
              <svg xmlns="http://www.w3.org/2000/svg" class="delete" width="22" height="22" fill="#aaa" viewBox="0 0 256 256"><path d="M216,48H180V36A28,28,0,0,0,152,8H104A28,28,0,0,0,76,36V48H40a12,12,0,0,0,0,24h4V208a20,20,0,0,0,20,20H192a20,20,0,0,0,20-20V72h4a12,12,0,0,0,0-24ZM100,36a4,4,0,0,1,4-4h48a4,4,0,0,1,4,4V48H100Zm88,168H68V72H188ZM116,104v64a12,12,0,0,1-24,0V104a12,12,0,0,1,24,0Zm48,0v64a12,12,0,0,1-24,0V104a12,12,0,0,1,24,0Z"></path></svg>
            </div>
          </div>
          <div class="main__div">
          <div class="workout__details">
            <span class="workout__icon">${
              workout.type === 'running' ? '🏃‍♂️' : '🚴‍♀️'
            }</span>
            <span class="workout__value">${workout.distance}</span>
            <span class="workout__unit">km</span>
          </div>
          <div class="workout__details">
            <span class="workout__icon">⏱</span>
            <span class="workout__value">${workout.duration}</span>
            <span class="workout__unit">min</span>
          </div>
`;
    if (workout.type === 'running') {
      html += `<div class="workout__details">
            <span class="workout__icon">⚡️</span>
            <span class="workout__value">${workout.pace.toFixed(1)}</span>
            <span class="workout__unit">min/km</span>
          </div>
          <div class="workout__details">
            <span class="workout__icon">🦶🏼</span>
            <span class="workout__value">${workout.cadence}</span>
            <span class="workout__unit">spm</span>
          </div>
          </div>
        </li>`;
    }
    if (workout.type === 'cycling') {
      html += `<div class="workout__details">
            <span class="workout__icon">⚡️</span>
            <span class="workout__value">${workout.speed.toFixed(1)}</span>
            <span class="workout__unit">km/h</span>
          </div>
          <div class="workout__details">
            <span class="workout__icon">⛰</span>
            <span class="workout__value">${workout.elevationGain}</span>
            <span class="workout__unit">m</span>
          </div>
          </div>
        </li>`;
    }
    form.insertAdjacentHTML('afterend', html);
  }

  _renderWorkoutMarker(workout) {
    L.marker(workout.coords)
      .addTo(this.#map)
      .bindPopup(
        L.popup({
          maxHeight: 100,
          maxWidth: 250,
          autoClose: false,
          closeOnClick: false,
          className: `${workout.type}-popup`,
        })
      )
      .setPopupContent(`${workout.description}`)
      .openPopup();
  }

  _deleteWorkout(e) {
    const workout = e.target.closest('.workout');
    // removing from the list
    workout.remove();

    // getting from local storage
    const data = JSON.parse(localStorage.getItem('workouts'));
    const item = data.findIndex(item => item.id === workout.dataset.id);

    // remove from map
    const markerCoords = data.find(coords => coords.id === workout.dataset.id);

    // removing from local storage
    data.splice(item, 1);
    localStorage.setItem('workouts', JSON.stringify(data));
  }

  // local storage
  _setLocalStorage() {
    localStorage.setItem('workouts', JSON.stringify(this.#workout));
  }
  _getLocalStorage() {
    this.#data = JSON.parse(localStorage.getItem('workouts'));

    if (!this.#data) return;

    this.#workout = this.#data; // cuz #workout is empty & _moveMap don't work onload
    this.#data.forEach(work => this._renderWorkout(work));
  }
  _getMarker(data) {
    if (!data) return;
    data.forEach(work => this._renderWorkoutMarker(work));
  }
}
const app = new App();4
```

# Promisifying geolocation API
```jsx
const showCountry = function () {
  return new Promise((response, reject) => {
    navigator.geolocation.getCurrentPosition(response, reject);
  });
};

const whereAmI = function () {
  showCountry()
    .then(loc => {
      const { latitude, longitude } = loc.coords;
      return fetch(
        `https://geocode.xyz/${latitude},${longitude}?geoit=json&auth=831253157316286630930x110214 `
      );
    })
    .then(response => response.json())
    .then(jason => {
      const countryCode = jason.prov;
      return fetch(`https://restcountries.com/v3.1/alpha/${countryCode}`);
    })
    .then(res => res.json())
    .then(jason => {
      const [data] = jason;
      renderCountry(data);
    });
};
whereAmI();```
