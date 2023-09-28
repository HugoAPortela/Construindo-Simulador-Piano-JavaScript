# Paino

[![npm version](https://img.shields.io/npm/v/react-piano.svg)](https://www.npmjs.com/package/react-piano)
[![build status](https://travis-ci.com/kevinsqi/react-piano.svg?branch=master)](https://travis-ci.com/kevinsqi/react-piano)
[![bundle size](https://img.shields.io/bundlephobia/min/react-piano.svg)](https://bundlephobia.com/result?p=react-piano)

Um teclado de piano interativo para React. Suporta sons personalizados, eventos de toque/clique/teclado e estilo totalmente configurável. Experimente no CodeSandbox.

<a href="https://www.kevinqi.com/react-piano/"><img width="600" src="/demo/public/images/react-piano-screenshot.png" alt="react-piano screenshot" /></a>

## Instalando

```
yarn add react-piano
```

Alternativamente, você pode baixar a compilação UMD em unpkg.

## Uso

Você pode visualizar ou bifurcar a demonstração do CodeSandbox para obter uma versão ao vivo do componente em ação.

Importe o componente e os estilos:

```jsx
import { Piano, KeyboardShortcuts, MidiNumbers } from 'react-piano';
import 'react-piano/dist/styles.css';
```

A importação de CSS requer um carregador de CSS (se você estiver usando create-react-app, isso já está configurado para você). Se você não possui um carregador CSS, você pode copiar o arquivo CSS para o seu projeto em src/styles.css .

Então, para usar o componente:


function App() {
  const firstNote = MidiNumbers.fromNote('c3');
  const lastNote = MidiNumbers.fromNote('f5');
  const keyboardShortcuts = KeyboardShortcuts.create({
    firstNote: firstNote,
    lastNote: lastNote,
    keyboardConfig: KeyboardShortcuts.HOME_ROW,
  });

  return (
    <Piano
      noteRange={{ first: firstNote, last: lastNote }}
      playNote={(midiNumber) => {
        // Play a given note - see notes below
      }}
      stopNote={(midiNumber) => {
        // Stop playing a given note - see notes below
      }}
      width={1000}
      keyboardShortcuts={keyboardShortcuts}
    />
  );
}
```

## Implementando a reprodução de áudio

O react-piano não implementa a reprodução de áudio de cada nota, então você deve implementá-lo com playNotee stopNoteadereços. Isso lhe dá a capacidade de usar qualquer som que desejar com o piano renderizado. A página de demonstração do react-piano usa o excelente reprodutor de fontes sonoras do @danigb para reproduzir amostras de fontes sonoras com som realista. Dê uma olhada na demonstração do CodeSandbox para ver como você pode implementar isso sozinho.

## Adereços

| Name | Type | Description |
| ---- | ---- | ----------- |
| `noteRange` | **Required** object | An object with format `{ first: 48, last: 77 }` where first and last are MIDI numbers that correspond to natural notes. You can use `MidiNumbers.NATURAL_MIDI_NUMBERS` to identify whether a number is a natural note or not. |
| `playNote` | **Required** function | `(midiNumber) => void` function to play a note specified by MIDI number. |
| `stopNote` | **Required** function | `(midiNumber) => void` function to stop playing a note. |
| `width` | **Conditionally required** number | Width in pixels of the component. While this is not strictly required, if you omit it, the container around the `<Piano>` will need to have an explicit width and height in order to render correctly. |
| `activeNotes` | Array of numbers | An array of MIDI numbers, e.g. `[44, 47, 54]`, which allows you to programmatically play notes on the piano. |
| `keyWidthToHeight` | Number | Ratio of key width to height. Used to specify the dimensions of the piano key. |
| `renderNoteLabel` | Function | `({ keyboardShortcut, midiNumber, isActive, isAccidental }) => node` function to render a label on piano keys that have keyboard shortcuts |
| `className` | String | A className to add to the component. |
| `disabled` | Boolean | Whether to show disabled state. Useful when audio sounds need to be asynchronously loaded. |
| `keyboardShortcuts` | Array of object | An array of form `[{ key: 'a', midiNumber: 48 }, ...]`, where `key` is a `keyEvent.key` value. You can generate this using `KeyboardShortcuts.create`, or use your own method to generate it. You can omit it if you don't want to use keyboard shortcuts. **Note:** this shouldn't be generated inline in JSX because it can cause problems when diffing for shortcut changes. |
| `onPlayNoteInput` | Function | `(midiNumber, { prevActiveNotes }) => void` function that fires whenever a play-note event is fired. Can use `prevActiveNotes` to record notes. |
| `onStopNoteInput` | Function | `(midiNumber, { prevActiveNotes }) => void` function that fires whenever a stop-note event is fired. Can use `prevActiveNotes` to record notes. |

## Recording/saving notes

You can "record" notes that are played on a `<Piano>` by using `onPlayNoteInput` or `onStopNoteInput`, and you can then play back the recording by using `activeNotes`. See [this CodeSandbox](https://codesandbox.io/s/l4jjvzmp47) which demonstrates how to set that up.

<a href="https://codesandbox.io/s/l4jjvzmp47"><img width="300" src="/demo/public/images/recording-demo.gif" alt="demo of recording" /></a>

## Customizing styles

You can customize many aspects of the piano using CSS. In javascript, you can override the base styles by creating your own set of overrides:

```javascript
import 'react-piano/dist/styles.css';
import './customPianoStyles.css';  // import a set of overrides
```

In the CSS file you can do things like:

```css
.ReactPiano__Key--active {
  background: #f00;  /* Change the default active key color to bright red */
}

.ReactPiano__Key--accidental {
  background: #000;  /* Change accidental keys to be completely black */
}
```

See [styles.css](/src/styles.css) for more detail on what styles can be customized.

## Upgrading versions

See the [CHANGELOG](CHANGELOG.md) which contains migration guides for instructions on upgrading to each major version.

## Browser compatibility

To support IE, you'll need to provide an `Array.find` polyfill.

## License

MIT
