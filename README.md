Here's a README for the Password Generator project:

# Password Generator

A secure password generator web application that creates strong, customizable passwords based on user preferences.

![Password Generator Preview](https://password-generator-virid-ten.vercel.app/)

## Features

- Generate passwords between 4-20 characters
- Customizable character options:
  - Uppercase letters (A-Z)
  - Lowercase letters (a-z) 
  - Numbers (0-9)
  - Special characters (!@#$%^&*())
- Password strength indicator
- One-click copy to clipboard
- Responsive design for all screen sizes
- Visual feedback for user interactions

## Password Strength Calculation

The password strength is calculated using password entropy, which considers both the length of the password and the size of the character pool. The strength levels are:

- Too Weak (< 25 bits)
- Weak (25-49 bits)
- Medium (50-74 bits)
- Strong (≥ 75 bits)

Reference to strength calculation code:

```94:106:js/script.js
const calcStrength = (passwordLength, charPoolSize) => {
  const strength = passwordLength * Math.log2(charPoolSize);

  if(strength < 25) {
    return ['Too Weak!', 1];
  } else if (strength >= 25 && strength < 50) {
    return ['Weak', 2];
  } else if (strength >= 50 && strength < 75) {
    return ['Medium', 3];
  } else {
    return ['Strong', 4];
  }
}
```


## Technologies Used

- HTML5
- CSS3
- Vanilla JavaScript
- JetBrains Mono font

## CSS Features

- Custom styled range input slider
- Custom checkbox styling
- CSS Grid and Flexbox layouts
- Mobile-first responsive design
- Smooth transitions and hover effects

## JavaScript Functionality

Key implementations include:

1. Password Generation:

```108:144:js/script.js
const generatePassword = (e) => {
  e.preventDefault();
  try {
    validateInput();

    let generatedPassword = '';
    let includedSets = [];
    let charPool = 0;

    checkBoxes.forEach(box => {
      if(box.checked) {
        includedSets.push(CHARACTER_SETS[box.value][0]);
        charPool += CHARACTER_SETS[box.value][1];
      }
    });

    if (includedSets) {
      for(let i=0; i<lengthSlider.value; i++) {
        const randSetIndex = Math.floor(Math.random() * includedSets.length);
        const randSet = includedSets[randSetIndex];

        const randCharIndex = Math.floor(Math.random() * randSet.length);
        const randChar = randSet[randCharIndex];
        
        generatedPassword += randChar;
      }
    }
    
    const strength = calcStrength(lengthSlider.value, charPool);
    styleMeter(strength);

    passwordDisplay.textContent = generatedPassword;
    canCopy = true;
  } catch(err) {
    console.log(err);
  }
}
```


2. Copy to Clipboard:

```158:178:js/script.js
const copyPassword = async () => {
  if(!passwordDisplay.textContent || passwordCopiedNotification.textContent) return;
  if(!canCopy) return;

  await navigator.clipboard.writeText(passwordDisplay.textContent);
  passwordCopiedNotification.textContent = 'Copied';

  // Fade out text after 1 second
  setTimeout(() => {
    passwordCopiedNotification.style.transition = 'all 1s';
    passwordCopiedNotification.style.opacity = 0;

    // Remove styles and text after fade out
    setTimeout(() => {
      passwordCopiedNotification.style.removeProperty('opacity');
      passwordCopiedNotification.style.removeProperty('transition');
      passwordCopiedNotification.textContent = '';
    }, 1000);
  }, 1000);
  
}
```


## Responsive Design

The application is fully responsive and optimized for:
- Desktop (> 576px)
- Tablet (≤ 576px)
- Mobile (≤ 432px)
- Small Mobile (≤ 384px)

Reference to media queries:

```1:80:css/queries.css
@media (max-width: 36em) {
  .container {
    width: 91.6%;
  }

  p {
    font-size: 1.6rem;
  }

  .password-display,
  .password-placeholder {
    font-size: 2.4rem;
  }

  .password-field {
    height: 64px;
    margin-bottom: 1.6rem;
  }

  .char-count {
    font-size: 2.8rem;
  }

  .strength-box {
    height: 56px;
  }
}

@media (max-width: 27em) {
  .password-settings {
    padding: 2.4rem 1.6rem 3.2rem;
  }
  
  .strength-rating-text {
    font-size: 1.6rem;
  }
}

@media (max-width: 24em) {
  h1 {
    font-size: 1.6rem;
    margin-bottom: 1.6rem;
  }

  .password-settings {
    padding: 1.6rem
  }

  .char-length {
    margin-bottom: 1.6rem;
  }

  .char-length-slider {
    height: 8px;
    margin-bottom: 4.2rem;
  }

  .char-count {
    font-size: 2.4rem;
  }

  .char-include-group {
    gap: 1.6rem;
    margin-bottom: 3.2rem;
  }

  .strength-box {
    margin-bottom: 1.6rem;
    padding: 0 1.6rem;
  }

  .strength-label {
    font-size: 1.8rem;
  }

  .generate-btn {
    padding: 1.5rem 0;
    font-size: 1.6rem;
  }
}
```


## Setup

1. Clone the repository
2. Open `index.html` in your browser
3. No build process or dependencies required

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## License

This project uses the JetBrains Mono font under the OFL (Open Font License).

## Credits

- Design inspiration: Frontend Mentor
- Font: JetBrains Mono
