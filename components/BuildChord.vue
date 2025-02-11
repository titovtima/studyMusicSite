<template>
  <div style="padding: 2rem; display: flex; align-items: center; flex-direction: column;">
    <NuxtLink v-if="route.path != '/chords'" to="/chords"><h1 style="margin-bottom: 0;">Построить аккорд</h1></NuxtLink>
    <h1 v-else style="margin-bottom: 0;">Построить аккорд</h1>
    <div>
      <label>Нота:</label>
      <input ref="noteInput" list="chooseNoteDatalist"/>
      <datalist id="chooseNoteDatalist">
        <option v-for="note of notes">{{ note }}</option>
      </datalist>
      <label>Тип:</label>
      <input ref="chordTypeInput" list="chooseChordTypeDatalist"/>
      <datalist id="chooseChordTypeDatalist">
        <option v-for="chordType of chordTypes">{{ chordType }}</option>
      </datalist>
    </div>
    <div>или</div>
    <div style="margin-bottom: 1rem;">
      <label>Аккорд целиком:</label>
      <input ref="chordInput" list="chooseChordDatalist"/>
      <datalist id="chooseChordDatalist">
        <option v-for="chord of allChords">{{ chord }}</option>
      </datalist>
    </div>
    <div>
      <Piano :octaves="octaves" :pressed="pressed"/>
    </div>
  </div>
</template>

<script setup lang="ts">
import musicTheory from '@titovtima/music-theory';

const noteInput: Ref<any> = ref(null);
const chordTypeInput: Ref<any> = ref(null);
const chordInput: Ref<any> = ref(null);

const route = useRoute();

let notes = ['C', 'C♭', 'C♯', 'D♭', 'D', 'D♯', 'E♭', 'E', 'E♯', 'F♭', 'F', 'F♯', 'G♭', 'G', 'G♯', 'A♭', 'A', 'A♯', 'B♭', 'B', 'B♯'];
let chordTypes = musicTheory.allChordTypes().map(value => value.name);

let allChords: string[] = [];
for (let note of notes) {
  for (let type of chordTypes) {
    allChords.push(note + type);
  }
}

const octaves = ref(1);
const pressed: Ref<number[]> = ref([]);

onMounted(() => {
  noteInput.value.oninput = () => { buildChord(); };
  chordTypeInput.value.oninput = () => { buildChord(); };
  chordInput.value.oninput = () => { buildChord(false); };
});

function buildChord(fromNoteInput: boolean = true) {
  if (fromNoteInput) {
    chordInput.value.value = noteInput.value.value + chordTypeInput.value.value;
  } else {
    let note = musicTheory.noteFromString(chordInput.value.value.replace(/#/, '♯').replace(/b/, '♭'));
    let noteString = '';
    if (note != undefined)
      noteString = musicTheory.noteName(note);
    noteInput.value.value = noteString;
    chordTypeInput.value.value = chordInput.value.value.substr(noteString.length);
  }
  let chordString = chordInput.value.value.replace(/#/, '♯').replace(/b/, '♭');
  if (allChords.includes(chordString)) {
    try {
      let chord = musicTheory.chordFromName(chordString, 'English');
      let chordNotes = musicTheory.getChordNotes(chord);
      pressed.value = chordNotes.map(note => note.noteId);
      octaves.value = Math.max(1, ...chordNotes.map(note => Math.floor(note.noteId / 12) + 1));
    } catch(e) {
      pressed.value = [];
      octaves.value = 1;
    }
  } else {
    pressed.value = [];
    octaves.value = 1;
  }
}
</script>

<style scoped>
input {
  border: 1px solid #000;
  margin: 0 0.5rem 0 0;
}
</style>
