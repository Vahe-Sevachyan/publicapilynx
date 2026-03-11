# Vue 3 Event Handling - Composition API Guide

## Table of Contents

1. Basic Event Binding
2. Event Modifiers
3. Key Modifiers
4. Mouse Button Modifiers
5. Accessing Event Arguments
6. Inline Handlers vs Method Handlers
7. Multiple Event Handlers
8. Custom Events with defineEmits
9. Event Validation
10. Real-World Examples

---

## 1. Basic Event Binding

Use `v-on` directive or the `@` shorthand to listen to DOM events.

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

function handleClick() {
  count.value++
}

function handleDoubleClick() {
  count.value += 2
}
</script>

<template>
  <div>
    <!-- Long form -->
    <button v-on:click="handleClick">v-on syntax</button>

    <!-- Shorthand (preferred) -->
    <button @click="handleClick">@ syntax</button>

    <!-- Multiple event types -->
    <button @click="handleClick" @dblclick="handleDoubleClick">Count: {{ count }}</button>
  </div>
</template>
```

---

## 2. Event Modifiers

Event modifiers allow you to handle common event manipulations declaratively.

### Available Modifiers

```vue
<script setup>
import { ref } from 'vue'

const message = ref('')

function handleSubmit() {
  message.value = 'Form submitted!'
}

function handleClick() {
  message.value = 'Clicked!'
}

function handleChildClick() {
  message.value = 'Child clicked!'
}
</script>

<template>
  <div>
    <!-- .prevent - calls event.preventDefault() -->
    <form @submit.prevent="handleSubmit">
      <button type="submit">Submit (no page reload)</button>
    </form>

    <!-- .stop - calls event.stopPropagation() -->
    <div @click="handleClick">
      Parent
      <button @click.stop="handleChildClick">Child (won't trigger parent)</button>
    </div>

    <!-- .self - only trigger if event.target is the element itself -->
    <div @click.self="handleClick">
      Only triggers when clicking this div, not children
      <button>This won't trigger parent handler</button>
    </div>

    <!-- .capture - use capture mode -->
    <div @click.capture="handleClick">Triggers during capture phase</div>

    <!-- .once - event will trigger at most once -->
    <button @click.once="handleClick">Click me (only works once)</button>

    <!-- .passive - improves scrolling performance -->
    <div @scroll.passive="handleScroll">Scrollable content</div>

    <!-- Chain modifiers (order matters for some) -->
    <form @submit.prevent.stop="handleSubmit">
      <button type="submit">Submit</button>
    </form>
  </div>
</template>
```

### Modifier Order Matters

```vue
<template>
  <!-- Prevents all clicks -->
  <button @click.prevent.self="handleClick">...</button>

  <!-- Only prevents clicks on the element itself -->
  <button @click.self.prevent="handleClick">...</button>
</template>
```

---

## 3. Key Modifiers

Listen for specific keyboard keys.

```vue
<script setup>
import { ref } from 'vue'

const searchQuery = ref('')
const messages = ref([])

function handleEnter() {
  if (searchQuery.value.trim()) {
    messages.value.push(searchQuery.value)
    searchQuery.value = ''
  }
}

function handleEscape() {
  searchQuery.value = ''
}

function handleDelete() {
  console.log('Delete pressed')
}

function handleCtrlS(event) {
  event.preventDefault()
  console.log('Saving...')
}
</script>

<template>
  <div>
    <!-- Specific key modifiers -->
    <input
      v-model="searchQuery"
      @keyup.enter="handleEnter"
      @keyup.esc="handleEscape"
      @keyup.delete="handleDelete"
      placeholder="Press Enter to submit, Esc to clear"
    />

    <!-- Key combinations -->
    <input @keydown.ctrl.s="handleCtrlS" placeholder="Ctrl+S to save" />

    <!-- System modifier keys -->
    <button @click.ctrl="handleClick">Ctrl + Click</button>
    <button @click.shift="handleClick">Shift + Click</button>
    <button @click.alt="handleClick">Alt + Click</button>
    <button @click.meta="handleClick">Cmd/Win + Click</button>

    <!-- .exact modifier - requires exact key combination -->
    <button @click.ctrl.exact="handleClick">Only Ctrl (no Shift, Alt, or Meta)</button>

    <!-- Common key aliases -->
    <!-- .enter, .tab, .delete, .esc, .space -->
    <!-- .up, .down, .left, .right -->
    <!-- .ctrl, .alt, .shift, .meta -->
  </div>
</template>
```

### Custom Key Modifiers

```vue
<script setup>
function handlePageDown() {
  console.log('Page Down pressed')
}
</script>

<template>
  <!-- Use kebab-case for multi-word keys -->
  <input @keyup.page-down="handlePageDown" />
</template>
```

---

## 4. Mouse Button Modifiers

Handle specific mouse buttons.

```vue
<script setup>
import { ref } from 'vue'

const action = ref('')

function handleLeftClick() {
  action.value = 'Left click'
}

function handleRightClick() {
  action.value = 'Right click'
}

function handleMiddleClick() {
  action.value = 'Middle click'
}
</script>

<template>
  <div>
    <div
      @click.left="handleLeftClick"
      @click.right.prevent="handleRightClick"
      @click.middle="handleMiddleClick"
      style="padding: 20px; border: 1px solid #ccc;"
    >
      Try different mouse buttons: {{ action }}
    </div>
  </div>
</template>
```

---

## 5. Accessing Event Arguments

### Method Handlers

```vue
<script setup>
import { ref } from 'vue'

const lastEvent = ref(null)

function handleClick(event) {
  lastEvent.value = {
    type: event.type,
    target: event.target.tagName,
    clientX: event.clientX,
    clientY: event.clientY,
  }
}

function handleInput(event) {
  console.log('Input value:', event.target.value)
}
</script>

<template>
  <div>
    <button @click="handleClick">Click me</button>
    <input @input="handleInput" />

    <pre v-if="lastEvent">{{ lastEvent }}</pre>
  </div>
</template>
```

### Inline Handlers with Arguments

```vue
<script setup>
import { ref } from 'vue'

const messages = ref([])

function addMessage(text, event) {
  messages.value.push({
    text,
    timestamp: new Date().toISOString(),
    target: event.target.tagName,
  })
}

function removeItem(index, event) {
  event.stopPropagation()
  messages.value.splice(index, 1)
}
</script>

<template>
  <div>
    <!-- Pass custom arguments -->
    <button @click="addMessage('Hello', $event)">Say Hello</button>
    <button @click="addMessage('Goodbye', $event)">Say Goodbye</button>

    <!-- Access event with $event in inline handlers -->
    <button @click="(event) => addMessage('Inline', event)">Inline Handler</button>

    <!-- Multiple arguments -->
    <ul>
      <li v-for="(msg, index) in messages" :key="index">
        {{ msg.text }}
        <button @click="removeItem(index, $event)">Remove</button>
      </li>
    </ul>
  </div>
</template>
```

---

## 6. Inline Handlers vs Method Handlers

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

function incrementCount() {
  count.value++
}

function decrementCount() {
  count.value--
}
</script>

<template>
  <div>
    <!-- Method handler (preferred for reusability) -->
    <button @click="incrementCount">Increment</button>

    <!-- Inline handler (good for simple operations) -->
    <button @click="count++">Increment Inline</button>

    <!-- Inline handler with multiple statements -->
    <button
      @click="
        count--
        console.log('Decremented')
      "
    >
      Decrement & Log
    </button>

    <!-- Inline with method call -->
    <button
      @click="
        decrementCount()
        console.log(count)
      "
    >
      Decrement (Method)
    </button>

    Count: {{ count }}
  </div>
</template>
```

---

## 7. Multiple Event Handlers

```vue
<script setup>
import { ref } from 'vue'

const logs = ref([])

function logAction(action) {
  logs.value.push(`${action} at ${new Date().toLocaleTimeString()}`)
}

function trackAnalytics() {
  console.log('Analytics tracked')
}

function validateInput() {
  console.log('Input validated')
}
</script>

<template>
  <div>
    <!-- Multiple handlers for same event (Vue 3.x) -->
    <button @click="logAction('Button clicked')" @click="trackAnalytics">
      Multi-handler button
    </button>

    <!-- Calling multiple functions in one handler -->
    <button
      @click="
        () => {
          logAction('Saved')
          trackAnalytics()
        }
      "
    >
      Save
    </button>
  </div>
</template>
```

---

## 8. Custom Events with defineEmits

### Child Component

```vue
<!-- ChildComponent.vue -->
<script setup>
const emit = defineEmits(['update', 'delete', 'save'])

function handleUpdate() {
  emit('update', { id: 1, name: 'Updated Item' })
}

function handleDelete() {
  emit('delete', 1)
}

function handleSave(data) {
  emit('save', data)
}
</script>

<template>
  <div>
    <button @click="handleUpdate">Update</button>
    <button @click="handleDelete">Delete</button>
    <button @click="handleSave({ name: 'New Data' })">Save</button>
  </div>
</template>
```

### Parent Component

```vue
<!-- ParentComponent.vue -->
<script setup>
import { ref } from 'vue'
import ChildComponent from './ChildComponent.vue'

const items = ref([])

function handleUpdate(item) {
  console.log('Item updated:', item)
  items.value.push(item)
}

function handleDelete(id) {
  console.log('Item deleted:', id)
  items.value = items.value.filter((item) => item.id !== id)
}

function handleSave(data) {
  console.log('Data saved:', data)
}
</script>

<template>
  <ChildComponent @update="handleUpdate" @delete="handleDelete" @save="handleSave" />
</template>
```

---

## 9. Event Validation

Validate emitted events with TypeScript-style syntax.

```vue
<script setup>
// With validation
const emit = defineEmits({
  // Basic validation
  submit: null,

  // Validate payload
  update: (payload) => {
    if (typeof payload.id === 'number' && payload.name) {
      return true
    }
    console.warn('Invalid update payload')
    return false
  },

  // Multiple arguments
  delete: (id, reason) => {
    return typeof id === 'number' && typeof reason === 'string'
  },
})

function handleSubmit() {
  emit('submit')
}

function handleUpdate() {
  emit('update', { id: 1, name: 'Valid' }) // Valid
  emit('update', { id: 'invalid' }) // Invalid - warning in console
}

function handleDelete() {
  emit('delete', 1, 'User requested') // Valid
  emit('delete', '1', 'Reason') // Invalid
}
</script>

<template>
  <div>
    <button @click="handleSubmit">Submit</button>
    <button @click="handleUpdate">Update</button>
    <button @click="handleDelete">Delete</button>
  </div>
</template>
```

### TypeScript Support

```vue
<script setup lang="ts">
interface UpdatePayload {
  id: number
  name: string
}

const emit = defineEmits<{
  update: [payload: UpdatePayload]
  delete: [id: number, reason: string]
  submit: []
}>()

function handleUpdate() {
  emit('update', { id: 1, name: 'Item' }) // Type-safe
}
</script>
```

---

## 10. Real-World Examples

### Example 1: Search Component with Debouncing

```vue
<script setup>
import { ref, watch } from 'vue'

const emit = defineEmits(['search'])
const searchQuery = ref('')
const isSearching = ref(false)
let debounceTimer = null

function handleInput(event) {
  clearTimeout(debounceTimer)
  isSearching.value = true

  debounceTimer = setTimeout(() => {
    performSearch(event.target.value)
  }, 500)
}

function performSearch(query) {
  if (query.trim()) {
    emit('search', query)
  }
  isSearching.value = false
}

function handleClear() {
  searchQuery.value = ''
  emit('search', '')
}

function handleSubmit() {
  clearTimeout(debounceTimer)
  performSearch(searchQuery.value)
}
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <input
      v-model="searchQuery"
      @input="handleInput"
      @keyup.esc="handleClear"
      placeholder="Search... (Esc to clear)"
      type="text"
    />
    <button type="submit">Search</button>
    <button type="button" @click="handleClear">Clear</button>
    <span v-if="isSearching">Searching...</span>
  </form>
</template>
```

### Example 2: Modal Component

```vue
<script setup>
import { onMounted, onUnmounted } from 'vue'

const props = defineProps({
  show: Boolean,
})

const emit = defineEmits(['close', 'confirm'])

function handleClose() {
  emit('close')
}

function handleConfirm() {
  emit('confirm')
  emit('close')
}

function handleEscape(event) {
  if (event.key === 'Escape' && props.show) {
    handleClose()
  }
}

function handleBackdropClick(event) {
  if (event.target === event.currentTarget) {
    handleClose()
  }
}

onMounted(() => {
  document.addEventListener('keydown', handleEscape)
})

onUnmounted(() => {
  document.removeEventListener('keydown', handleEscape)
})
</script>

<template>
  <Transition name="modal">
    <div v-if="show" class="modal-backdrop" @click="handleBackdropClick">
      <div class="modal-content" @click.stop>
        <slot />
        <div class="modal-actions">
          <button @click="handleClose">Cancel</button>
          <button @click="handleConfirm">Confirm</button>
        </div>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  max-width: 500px;
}
</style>
```

### Example 3: Form with Validation

```vue
<script setup>
import { reactive, ref } from 'vue'

const emit = defineEmits(['submit'])

const form = reactive({
  email: '',
  password: '',
  confirmPassword: '',
})

const errors = reactive({
  email: '',
  password: '',
  confirmPassword: '',
})

const isSubmitting = ref(false)

function validateEmail() {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!form.email) {
    errors.email = 'Email is required'
  } else if (!emailRegex.test(form.email)) {
    errors.email = 'Invalid email format'
  } else {
    errors.email = ''
  }
}

function validatePassword() {
  if (!form.password) {
    errors.password = 'Password is required'
  } else if (form.password.length < 8) {
    errors.password = 'Password must be at least 8 characters'
  } else {
    errors.password = ''
  }
}

function validateConfirmPassword() {
  if (form.password !== form.confirmPassword) {
    errors.confirmPassword = 'Passwords do not match'
  } else {
    errors.confirmPassword = ''
  }
}

function handleSubmit() {
  validateEmail()
  validatePassword()
  validateConfirmPassword()

  const hasErrors = Object.values(errors).some((error) => error !== '')

  if (!hasErrors) {
    isSubmitting.value = true
    emit('submit', { ...form })

    // Reset form
    setTimeout(() => {
      form.email = ''
      form.password = ''
      form.confirmPassword = ''
      isSubmitting.value = false
    }, 1000)
  }
}

function handleReset() {
  form.email = ''
  form.password = ''
  form.confirmPassword = ''
  errors.email = ''
  errors.password = ''
  errors.confirmPassword = ''
}
</script>

<template>
  <form @submit.prevent="handleSubmit" @reset.prevent="handleReset">
    <div>
      <label>Email:</label>
      <input
        v-model="form.email"
        @blur="validateEmail"
        type="email"
        :class="{ error: errors.email }"
      />
      <span class="error-message">{{ errors.email }}</span>
    </div>

    <div>
      <label>Password:</label>
      <input
        v-model="form.password"
        @blur="validatePassword"
        type="password"
        :class="{ error: errors.password }"
      />
      <span class="error-message">{{ errors.password }}</span>
    </div>

    <div>
      <label>Confirm Password:</label>
      <input
        v-model="form.confirmPassword"
        @blur="validateConfirmPassword"
        @keyup.enter="handleSubmit"
        type="password"
        :class="{ error: errors.confirmPassword }"
      />
      <span class="error-message">{{ errors.confirmPassword }}</span>
    </div>

    <div class="actions">
      <button type="submit" :disabled="isSubmitting">
        {{ isSubmitting ? 'Submitting...' : 'Submit' }}
      </button>
      <button type="reset">Reset</button>
    </div>
  </form>
</template>

<style scoped>
.error {
  border-color: red;
}

.error-message {
  color: red;
  font-size: 12px;
}
</style>
```

### Example 4: Drag and Drop

```vue
<script setup>
import { ref } from 'vue'

const emit = defineEmits(['filesDropped'])

const isDragging = ref(false)
const files = ref([])

function handleDragEnter(event) {
  event.preventDefault()
  isDragging.value = true
}

function handleDragLeave(event) {
  event.preventDefault()
  isDragging.value = false
}

function handleDragOver(event) {
  event.preventDefault()
}

function handleDrop(event) {
  event.preventDefault()
  isDragging.value = false

  const droppedFiles = Array.from(event.dataTransfer.files)
  files.value = droppedFiles
  emit('filesDropped', droppedFiles)
}

function handleFileInput(event) {
  const selectedFiles = Array.from(event.target.files)
  files.value = selectedFiles
  emit('filesDropped', selectedFiles)
}

function removeFile(index) {
  files.value.splice(index, 1)
}
</script>

<template>
  <div>
    <div
      class="drop-zone"
      :class="{ dragging: isDragging }"
      @dragenter="handleDragEnter"
      @dragleave="handleDragLeave"
      @dragover="handleDragOver"
      @drop="handleDrop"
    >
      <p>Drag and drop files here</p>
      <p>or</p>
      <input type="file" multiple @change="handleFileInput" />
    </div>

    <ul v-if="files.length">
      <li v-for="(file, index) in files" :key="index">
        {{ file.name }} ({{ (file.size / 1024).toFixed(2) }} KB)
        <button @click="removeFile(index)">Remove</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.drop-zone {
  border: 2px dashed #ccc;
  border-radius: 8px;
  padding: 40px;
  text-align: center;
  transition: all 0.3s;
}

.drop-zone.dragging {
  border-color: #4caf50;
  background: #f0f8f0;
}
</style>
```

---

## Key Takeaways

1. **Use `@` shorthand** for cleaner syntax
2. **Modifiers are chainable** - order matters for some combinations
3. **Named functions** for reusable handlers, **arrow functions** for inline callbacks
4. **`$event`** gives access to native event in inline handlers
5. **`defineEmits`** for type-safe custom events
6. **Event modifiers** handle common patterns (`.prevent`, `.stop`, `.once`)
7. **Validation** helps catch errors during development
8. **Combine modifiers** for complex interactions (`.ctrl.exact`, `.prevent.stop`)

This covers the complete event handling system in Vue 3 with Composition API. All examples use `<script setup>` syntax as required.
