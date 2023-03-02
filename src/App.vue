<template>
  <!-- <div id="root"> -->
    <div id="emoji-list">
    <h1>{{ header }}</h1>
    <!-- 
      v-model

      It does two-way data binding!
      Variants:
      - v-model.lazy - updates only when the input is blurred
      - v-model.number - casts that to number (by default it's string)
      - v-model.trim - automatically trims the string
    -->

    <!-- 
      v-on

      v-on directive handles user events.
      You choose which event you want to handle by adding its name after :
      - v-on:click - listens to click event
      - v-on:keyup.enter - listens to keyup event + allows you to choose which key triggers it

      We can replace v-on: with @ to make it shorter.
    -->
    <section class="section-emoji">
      Your suggestion:
      <input 
        v-model="newEmoji"
        v-on:keyup.enter="onAddEmoji"
        type="text"
        placeholder="Democratically suggest a new best emoji"
      />
      <!-- 
        Here we use v-model to bind one property to two radio boxes,
        and Vue automatically knows that to select one it needs to
        deselect the other - and it does it
        -->
      <label>
        <input v-model="newEmojiPriority" type="radio" value="high" />
        High
      </label>
      <label>
        <input v-model="newEmojiPriority" type="radio" value="low" />
        Low
      </label>
      <!-- 
        This is the shorthand for the v-on: directive.
        We can replace "v-on:" part with a single "@" and it works just the same.
       -->
      <button @click="onAddEmoji">Add</button>
    </section>

    <section>
      Other players' suggestions:
      <!-- 
        v-for

        It allows you to loop over data to render components.
        -->
      <label v-for="emoji in suggestedEmojis" :key="emoji.value">
        <input
          type="checkbox"
          v-model="suggestedEmojisChosen"
          :value="emoji.value"
          :key="emoji.value"
        >
        {{ emoji.label }}
      </label>
    </section>

    <section>
      <h2>Your ranking</h2>
      <!-- 
        v-if
        
        It allows you to render components conditionally.
        You can use a flat value here, or methods from either:
        - computed (you cannot pass params to it, but it's reactive) or
        - methods (you can pass params to it)
        -->
      <ol>
        <li v-for="{id, label} in items" :key="id">{{ label }}</li>
        <li v-for="chosenEmoji in suggestedEmojisChosen" :key="chosenEmoji">{{ chosenEmoji }}</li>
      </ol>
    </section>
  </div>
<!-- </div> -->
</template>

<script lang="ts">
export default {
  /**
   * ----- DATA -----
   * 
   * Data properties declared here can be used directly in the template.
   * You can also access them from the outside, for example in browser console.
   */
  data() {
    return {
      header: "Democratic list of the best vanilla emojis",
      newEmoji: '',
      newEmojiPriority: 'low',
      suggestedEmojis: [
        { value: "xd", label: "xd" },
        { value: ":|", label: ":|" },
        { value: ":^)", label: ":^)" }
      ],
      suggestedEmojisChosen: [],
      items: [
        {id: 1, label: ':3'},
        {id: 2, label: '>:c'},
        {id: 3, label: 'owo'},
      ]
    }
  },
  /**
   * ----- METHODS -----
   * 
   * In the methods we don't have direct access to data. We have to use "this" keyword.
   */
  methods: {
    showEmojiWhen(placement: string) {
      return this.newEmoji && this.newEmojiPriority === placement
    },
    onAddEmoji() {
      if (this.newEmojiPriority === 'low')
        this.items.push({id: this.items.length +1, label: this.newEmoji })
      if (this.newEmojiPriority === 'high')
        this.items.unshift({id: this.items.length +1, label: this.newEmoji })
    }
  }
}
</script>
