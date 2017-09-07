
<b><h1>Transcript a audio/video file using google cloud speech api </h1></b>
<p>
<ul>
<li>Create a gcp account.</li>
<li>Enable cloud speech api.</li>
<li>Remember to save your API_KEY and export it to your bin path using export API_KEY=XXXXXX.</li>
<li>Create a bucket in storage and upload your mono stereo .flac files.</li>
<li>Enable it to be accessible through public link. (Not ideal but its ok for learning purposes.)</li>
<br>
<br>
<b><h1>Create Transcript for the audio/video file</h1></b>
<ul>
<li>
If your file format is .mp4, convert it to .flac format with mono stereo.</li>
<li>You can convert the non mono stereo file to mono stereo using ffmpeg. Download and install it first.</li>
<li>Use the following command to convert.
ffmpeg -i location_of_your_file/audio_file.flac -ac 1 mono.flac</li>
<li>Open the .flac file using quicktime player and get its sample rate </li>
<li>create request.json file
{
  "config": {
      "encoding":"FLAC",  
      "sampleRateHertz": 44100,  
      "language_code": "en-US"
  },
  "audio": {
      "uri":"gs://speechtotextapi/mono.flac"
  }
}</li>

<li>If the audio/video content of the file is greater than 1 min use speech:longrunningrecognize endpoint, otherwise use speech:recognize.
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:longrunningrecognize?key=${API_KEY}"
</li>
<li>
https://speech.googleapis.com/v1/operations/OPERATION_NAME?key=${API_KEY} \
| jq -r '.response.results[].alternatives[].transcript' > output.txt
</li>
</ul>
