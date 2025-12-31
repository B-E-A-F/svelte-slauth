<script lang="ts">
import {Totp, OtpAlgorithm} from "@devolutions/slauth";

// Time to check and refresh to see if there is a new code
const INTERVAL = 1000;


type Config = {
  secret: string;
  period: number;
  digits: number;
  algo: OtpAlgorithm;
}

const config: Config = {
  secret: 'GEZDGNBVGY',
  period: 30,
  digits: 6,
  algo: OtpAlgorithm.sha256(),
}

// TODO: can I spread this
const totp = Totp.fromParts(config.secret, config.period, config.digits, config.algo)
let generatedCode = $state(totp.generateCode())

$effect(() => {
    // every period, run a timeout that updates the state
    setInterval(() => {
      const newCode = totp.generateCode();
      if (newCode !== generatedCode){
        console.log("New code generated:", newCode);
        generatedCode = newCode;
      }
    }, INTERVAL)
})

</script>

<div class="flex items-center justify-center h-screen w-screen bg-base-300">
  <div class="flex gap-4 items-center justify-center">
  
    <div class="mockup-phone w-72">
      <div class="mockup-phone-camera"></div>
      <div class="mockup-phone-display text-white grid place-content-center bg-neutral-900">
        {generatedCode}
      </div>
    </div>

    <div class="card card-side bg-base-100 shadow-sm w-lg">
      <figure>
        <img
          src="https://avatars.githubusercontent.com/u/6097418?s=200&v=4"
          alt="Movie" />
      </figure>
      <div class="card-body">
        <h2 class="card-title">New movie is released!</h2>
        <p>Click the button to watch on Jetflix app.</p>
        <div class="card-actions justify-end">
          <button class="btn btn-primary">Watch</button>
        </div>
      </div>
    </div>
  </div>
</div>
