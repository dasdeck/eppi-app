<template>
  <div contentEditable="true" @input="$emit('input', $refs.content.textContent)" ref="content">{{ value }}</div>
</template>

<script>
export default {
    props: {
        value: String
    }
}
</script>

