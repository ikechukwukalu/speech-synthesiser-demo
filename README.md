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
                1 Corinthians 7:5: Do not deprive each other except perhaps by mutual consent and for a time, so that
                you may devote
                yourselves to prayer. Then come together again so that Satan will not tempt you because of your lack of
                self-control.
            </p>
            <p align="center">
                John 15:12: My command is this: Love each other as I have loved you.
            </p>
            <p align="center">
                Hebrews 11:1: Now faith is the assurance of things hoped for, the conviction of things not seen.
            </p>
            <p align="center">
                The city of <a href="index.html">Corinth</a> in about the year 50 would have been the burgeoning capital
                of a Roman province in the Greek East. ... A great center. It had two ports. One on the Aegean side and
                one on the Adriatic side so that it served as one of the major crossroads for Roman shipping throughout
                the Mediterranean. So when Paul arrived in Corinth in the year 50, he would have come up the slopes to
                the center of the city and seen the rise of a great Roman splendor. A kind of monumental city built
                around the remains of the older Greek city, the center of which was the temple of Apollo with its great
                monolithic Ionic columns standing up above the rest of the city.
            </p>
        </div>
        <p>
        <div class="btn-group">
            <button type="button" class="btn btn-success btn-lg" onclick="speakResponse()">Play</button>
            <button type="button" class="btn btn-warning btn-lg" onclick="pauseSpeechText()">Pause</button>
            <button type="button" class="btn btn-primary btn-lg" onclick="resumeSpeechText()">Resume</button>
            <button type="button" class="btn btn-danger btn-lg" onclick="cancelSpeechText()">Stop</button>
        </div>
        </p>
    </div>
    <script>
        var handleSpeech = 0;

        function pauseSpeechText() {
            window.speechSynthesis.pause();
            removeBugFix();
        }

        function resumeSpeechText() {
            window.speechSynthesis.resume();
            applyBugFix();
        }

        function cancelSpeechText() {
            window.speechSynthesis.cancel();
        }

        function apiBugFix() {
            window.speechSynthesis.pause();
            window.speechSynthesis.resume();
        }

        function playSpeechText() {
            var voices = window.speechSynthesis.getVoices();
            var voices = [];
            var utterThis = new SpeechSynthesisUtterance();

            // voices[2] for Google UK English Female on Chrome
            utterThis.voice = voices[2];
            utterThis.pitch = 1; // 1 to 2
            utterThis.rate = 1; // 1 to 2
            utterThis.volume = 1; // 0 to 1
            utterThis.onstart = function(event) {
                applyBugFix();
                console.log("started");
            };
            utterThis.onend = function(event) {
                removeBugFix();
                console.log('Finished in ' + event.elapsedTime + ' seconds.');
            };
            utterThis.onerror = function(event) {
                removeBugFix();
                console.log('Errored ' + event);
            }
            utterThis.onpause = function(event) {
                console.log('Paused ' + event);

            }
            utterThis.onresume = function(event) {
                console.log('Resumed ' + event);

            }
            utterThis.onboundary = function(event) {
                console.log('onboundary ' + event);
            }

            return utterThis;
        }

        function speakResponse() {
            cancelSpeechText() // if it errors, this clears out the error.
            var elementTxt = document.querySelector('.txt');
            var text = getElementsText(elementTxt);

            var sentences = text.split(".");
            for (var i = 0; i < sentences.length; i++) {
                if(sentences[i].trim().length > 1) {
                    var toSay = playSpeechText();
                    toSay.text = sentences[i];
                    window.speechSynthesis.speak(toSay);
                }
            }
        }

        function getElementsText(elementTxt) {
            var kids = elementTxt.childNodes;
            var txt = "";
            for (let i = 0; i < kids.length; i++) {
                if (kids[i].nodeType === Node.TEXT_NODE)
                    txt += (kids[i].textContent).trim();
                else
                    txt += getElementsText(kids[i]);
            }
            return txt;
        }

        function applyBugFix() {
            handleSpeech = setInterval(() => {
                apiBugFix();
            }, 10000);
        }

        function removeBugFix() {
            clearInterval(handleSpeech);
            handleSpeech = 0;
        }
    </script>
</body>

</html>
```