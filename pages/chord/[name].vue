<template>
  <Navbar/>
  <Piano v-bind="$attrs" ref="piano" :octaves="octaves" :pressed="pressed" style="margin: auto;"/>
</template>

<script setup lang="ts">
import musicTheory from '@titovtima/music-theory';
const { chordFromName, getChordNotes } = musicTheory;

const piano: Ref<any> = ref(null);
const pressed: Ref<Array<Number>> = ref([]);

const octaves = ref(2);

const route = useRoute()
let chordName = route.params.name;

if (typeof chordName != 'string') {
  throw createError({
    statusCode: 404,
  });
}

chordName = chordName.replace(/sharp/, "♯");
chordName = chordName.replace(/flat/, "♭");

let chord: musicTheory.Chord;
try {
  chord = chordFromName(chordName, 'English');
} catch(e) {
  throw createError({
    statusCode: 404,
    message: 'Не удалось определить аккорд'
  });
}

let notes: musicTheory.Note[] = getChordNotes(chord);
pressed.value = notes.map(note => note.noteId);
octaves.value = Math.max(1, ...notes.map(note => Math.floor(note.noteId / 12) + 1));
</script>
