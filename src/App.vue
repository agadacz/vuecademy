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
    <section class="section-emoji">
      Your suggestion:
      <input v-model="newEmoji" type="text" placeholder="Democratically suggest a new best emoji" />
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
    </section>

    <section>
      Other players' suggestions:
      <!-- 
        v-for

        It allows you to loop over data to render components.
        -->
      <label v-for="emoji in suggestedEmojis" :key="emoji.value">
        <input type="checkbox" v-model="suggestedEmojisChosen" :value="emoji.value" :key="emoji.value">
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
        <li v-if="showEmojiWhen('high')">{{newEmoji}}</li>
        <li v-for="{id, label} in items" :key="id">{{ label }}</li>
        <li v-for="chosenEmoji in suggestedEmojisChosen" :key="chosenEmoji">{{ chosenEmoji }}</li>
        <li v-if="showEmojiWhen('low')">{{newEmoji}}</li>
      </ol>
    </section>
  </div>
<!-- </div> -->
</template>

<script lang="ts">
export default {
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
  methods: {
    showEmojiWhen(placement: string) {
      return this.newEmoji && this.newEmojiPriority === placement
    }
  }
}
</script>
