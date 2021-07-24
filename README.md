```
<!DOCTYPE html>
<html lang="en">

<head>
    <title>Speech synthesiser</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
</head>

<body>
    <div class="container">
        <div class="txt">
            <h1 align="center">Speech synthesiser</h1>
            <p align="center">
                <img src="https://via.placeholder.com/350" class="img-thumbnail">
            </p>
            <p align="center">
                1 Corinthians 7:5: Do not deprive each other except perhaps by mutual consent and for a time, so that you may devote
                yourselves to prayer. Then come together again so that Satan will not tempt you because of your lack of
                self-control.
            </p>
            <p align="center">
                John 15:12: My command is this: Love each other as I have loved you.
            </p>
            <p align="center">
                Hebrews 11:1: Now faith is the assurance of things hoped for, the conviction of things not seen.
            </p>
        </div>
        <p>
        <div class="btn-group">
            <button type="button" class="btn btn-success btn-lg" onclick="playSpeechText()">Play</button>
            <button type="button" class="btn btn-warning btn-lg" onclick="pauseSpeechText()">Pause</button>
            <button type="button" class="btn btn-primary btn-lg" onclick="resumeSpeechText()">Resume</button>
            <button type="button" class="btn btn-danger btn-lg" onclick="cancelSpeechText()">Stop</button>
        </div>
        </p>
    </div>
    <script>
        function pauseSpeechText() {
            window.speechSynthesis.pause();
        }

        function resumeSpeechText() {
            window.speechSynthesis.resume();
        }

        function cancelSpeechText() {
            window.speechSynthesis.cancel();
        }

        function apiBugFix() {
            pauseSpeechText();
            resumeSpeechText();
        }

        function playSpeechText() {
            cancelSpeechText();
            var elementTxt = document.querySelector('.txt');
            console.log(getElementsText(elementTxt));
            var textToSpeak = getElementsText(elementTxt);
            var voices = [];
            var synth = window.speechSynthesis;
            var utterThis = new SpeechSynthesisUtterance(textToSpeak);

            // voices[2] for Google UK English Female
            utterThis.voice = voices[2];
            utterThis.pitch = 1; // 1 to 2
            utterThis.rate = 1; // 1 to 2
            utterThis.volume = 1; // 0 to 1
            synth.speak(utterThis);

            utterThis.onpause = function (event) {
                var char = event.utterance.text.charAt(event.charIndex);
                console.log('Speech paused at character ' + event.charIndex + ' of "' +
                    event.utterance.text + '", which is "' + char + '".');
            }

            var handle = setInterval(() => {
                apiBugFix();
            }, 10000);

            utterThis.onend = function () {
                clearInterval(handle);
                handle = 0;
            }
        }

        function getElementsText(elementTxt) {
            var kids = elementTxt.childNodes;
            var txt = "";
            for (let i = 0; i < kids.length; i++) {
                if (kids[i].nodeType === Node.TEXT_NODE)
                    txt += kids[i].textContent;
                else
                    txt += getElementsText(kids[i]);
            }
            return txt;
        }

    </script>
</body>

</html>
```