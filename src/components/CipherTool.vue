```vue
<template>
  <ion-page>
    <ion-content :fullscreen="true">

      <div class="app-background">

        <!-- Header -->
        <div class="header-section">
          <div class="logo">
            <span>🔐</span>
          </div>

          <div>
            <h1>MjRamirez</h1>
            <p>Secure Cipher Tool</p>
          </div>
        </div>

        <!-- Main Card -->
        <div class="cipher-card">

          <div class="card-header">
            <div>
              <h2>Encryption Tool</h2>
              <p>Encrypt or decrypt your message securely.</p>
            </div>

            <div class="lock-icon">
              🔒
            </div>
          </div>

          <!-- Text Input -->
          <div class="input-group">

            <label>MESSAGE</label>

            <div class="input-wrapper textarea-wrapper">

              <ion-textarea
                v-model="text"
                placeholder="Enter your plaintext or ciphertext..."
                :auto-grow="true"
                rows="5"
              ></ion-textarea>

            </div>

          </div>

          <!-- Key -->
          <div class="input-group">

            <label>ENCRYPTION KEY</label>

            <div class="input-wrapper">

              <span class="key-icon">🔑</span>

              <ion-input
                v-model.number="key"
                type="number"
                placeholder="Enter key (example: 3)"
              ></ion-input>

            </div>

            <small>
              The key determines how many positions each letter will shift.
            </small>

          </div>

          <!-- Buttons -->
          <div class="action-buttons">

            <ion-button
              class="encrypt-button"
              expand="block"
              @click="encrypt"
            >
              <span class="button-icon">🔐</span>
              Encrypt Message
            </ion-button>

            <ion-button
              class="decrypt-button"
              expand="block"
              @click="decrypt"
            >
              <span class="button-icon">🔓</span>
              Decrypt Message
            </ion-button>

          </div>

          <!-- Result -->
          <div
            v-if="result"
            class="result-card"
          >

            <div class="result-header">

              <div>
                <span class="result-label">RESULT</span>
                <h3>Processed Message</h3>
              </div>

              <span class="success-icon">✓</span>

            </div>

            <div class="result-text">
              {{ result }}
            </div>

            <ion-button
              fill="clear"
              class="clear-button"
              @click="clearAll"
            >
              Clear
            </ion-button>

          </div>

        </div>

        <!-- Footer -->
        <div class="footer">

          <span>Caesar Cipher</span>

          <span class="dot">•</span>

          <span>Encryption & Decryption</span>

        </div>

      </div>

    </ion-content>
  </ion-page>
</template>


<script setup>

import {
  IonPage,
  IonContent,
  IonTextarea,
  IonInput,
  IonButton
} from '@ionic/vue'

import { ref } from 'vue'


const text = ref('')
const key = ref(3)
const result = ref('')


function caesarCipher(input, shift) {

  return input
    .split('')
    .map(character => {

      const code = character.charCodeAt(0)

      // Uppercase letters
      if (code >= 65 && code <= 90) {

        return String.fromCharCode(
          ((code - 65 + shift) % 26 + 26) % 26 + 65
        )

      }

      // Lowercase letters
      if (code >= 97 && code <= 122) {

        return String.fromCharCode(
          ((code - 97 + shift) % 26 + 26) % 26 + 97
        )

      }

      // Keep spaces, numbers and symbols unchanged
      return character

    })
    .join('')

}


function encrypt() {

  if (!text.value.trim()) {

    result.value = 'Please enter a message to encrypt.'

    return

  }

  result.value = caesarCipher(
    text.value,
    Number(key.value)
  )

}


function decrypt() {

  if (!text.value.trim()) {

    result.value = 'Please enter a ciphertext to decrypt.'

    return

  }

  result.value = caesarCipher(
    text.value,
    -Number(key.value)
  )

}


function clearAll() {

  text.value = ''
  key.value = 3
  result.value = ''

}

</script>


<style scoped>

/* ================================
   MAIN BACKGROUND
================================ */

.app-background {

  min-height: 100vh;

  background:
    radial-gradient(
      circle at top left,
      #243b68 0%,
      transparent 35%
    ),
    radial-gradient(
      circle at bottom right,
      #172554 0%,
      transparent 40%
    ),
    #080d1c;

  padding: 35px 20px;

  color: white;

}


/* ================================
   HEADER
================================ */

.header-section {

  max-width: 720px;

  margin: 0 auto 28px;

  display: flex;

  align-items: center;

  gap: 15px;

}


.logo {

  width: 58px;

  height: 58px;

  border-radius: 18px;

  display: flex;

  align-items: center;

  justify-content: center;

  background: linear-gradient(
    135deg,
    #3b82f6,
    #6366f1
  );

  box-shadow:
    0 10px 30px rgba(59,130,246,.3);

  font-size: 28px;

}


.header-section h1 {

  margin: 0;

  font-size: 27px;

  font-weight: 800;

  letter-spacing: -0.5px;

}


.header-section p {

  margin: 3px 0 0;

  color: #9ca3af;

  font-size: 14px;

}


/* ================================
   MAIN CARD
================================ */

.cipher-card {

  max-width: 720px;

  margin: auto;

  padding: 30px;

  border-radius: 25px;

  background: rgba(17, 24, 39, .88);

  border: 1px solid rgba(255,255,255,.08);

  box-shadow:
    0 25px 70px rgba(0,0,0,.4);

  backdrop-filter: blur(15px);

}


/* ================================
   CARD HEADER
================================ */

.card-header {

  display: flex;

  justify-content: space-between;

  align-items: center;

  margin-bottom: 30px;

}


.card-header h2 {

  margin: 0;

  font-size: 24px;

  font-weight: 750;

}


.card-header p {

  margin: 7px 0 0;

  color: #9ca3af;

  font-size: 14px;

}


.lock-icon {

  width: 50px;

  height: 50px;

  border-radius: 15px;

  background: rgba(59,130,246,.12);

  display: flex;

  align-items: center;

  justify-content: center;

  font-size: 23px;

}


/* ================================
   INPUTS
================================ */

.input-group {

  margin-bottom: 24px;

}


.input-group label {

  display: block;

  margin-bottom: 9px;

  color: #a5b4fc;

  font-size: 11px;

  font-weight: 800;

  letter-spacing: 1.2px;

}


.input-wrapper {

  display: flex;

  align-items: center;

  gap: 10px;

  min-height: 55px;

  padding: 0 15px;

  border-radius: 14px;

  background: #0b1224;

  border: 1px solid #26324a;

  transition: .2s;

}


.input-wrapper:focus-within {

  border-color: #4f8cff;

  box-shadow:
    0 0 0 3px rgba(79,140,255,.1);

}


.textarea-wrapper {

  padding: 10px 15px;

  align-items: flex-start;

}


ion-textarea {

  --color: #f8fafc;

  --placeholder-color: #64748b;

  width: 100%;

  font-size: 15px;

}


ion-input {

  --color: #f8fafc;

  --placeholder-color: #64748b;

  width: 100%;

  font-size: 15px;

}


.key-icon {

  font-size: 18px;

}


.input-group small {

  display: block;

  margin-top: 8px;

  color: #64748b;

  font-size: 11px;

}


/* ================================
   BUTTONS
================================ */

.action-buttons {

  display: grid;

  grid-template-columns: 1fr 1fr;

  gap: 12px;

  margin-top: 28px;

}


ion-button {

  --border-radius: 13px;

  height: 52px;

  font-weight: 700;

  text-transform: none;

}


.encrypt-button {

  --background: linear-gradient(
    135deg,
    #2563eb,
    #4f46e5
  );

  --background-hover: #2563eb;

  --box-shadow:
    0 10px 25px rgba(37,99,235,.25);

}


.decrypt-button {

  --background: linear-gradient(
    135deg,
    #059669,
    #10b981
  );

  --background-hover: #059669;

  --box-shadow:
    0 10px 25px rgba(16,185,129,.2);

}


.button-icon {

  margin-right: 7px;

}


/* ================================
   RESULT
================================ */

.result-card {

  margin-top: 28px;

  padding: 20px;

  border-radius: 17px;

  background: #0b1224;

  border: 1px solid rgba(16,185,129,.25);

}


.result-header {

  display: flex;

  justify-content: space-between;

  align-items: center;

}


.result-label {

  color: #34d399;

  font-size: 10px;

  font-weight: 800;

  letter-spacing: 1.3px;

}


.result-header h3 {

  margin: 4px 0 0;

  font-size: 16px;

}


.success-icon {

  width: 32px;

  height: 32px;

  border-radius: 50%;

  display: flex;

  align-items: center;

  justify-content: center;

  background: rgba(16,185,129,.15);

  color: #34d399;

  font-weight: bold;

}


.result-text {

  margin-top: 18px;

  padding: 16px;

  border-radius: 12px;

  background: #070d1b;

  border: 1px solid #1f2937;

  color: #e5e7eb;

  font-size: 16px;

  line-height: 1.6;

  word-break: break-word;

  white-space: pre-wrap;

}


.clear-button {

  --color: #94a3b8;

  height: 35px;

  margin-top: 5px;

}


/* ================================
   FOOTER
================================ */

.footer {

  max-width: 720px;

  margin: 22px auto 0;

  text-align: center;

  color: #64748b;

  font-size: 11px;

}


.dot {

  margin: 0 8px;

  color: #334155;

}


/* ================================
   MOBILE
================================ */

@media (max-width: 600px) {

  .app-background {

    padding: 25px 15px;

  }


  .cipher-card {

    padding: 22px 18px;

    border-radius: 21px;

  }


  .card-header h2 {

    font-size: 21px;

  }


  .action-buttons {

    grid-template-columns: 1fr;

  }


  .header-section h1 {

    font-size: 23px;

  }

}

</style>

