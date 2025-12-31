<script lang="ts">
import { Totp, OtpAlgorithm } from "@devolutions/slauth";
import { onMount } from "svelte";

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

const totp = Totp.fromParts(config.secret, config.period, config.digits, config.algo)
let generatedCode = $state(totp.generateCode())

/**
 * We need to go from the current time to how much time is left for the initial code.
 * We can mod by the interval and then use the modulous to determine the initial time left on our code.
 */
function timeRemainingInCurrentPeriod(){
  const periodInterval = config.period * 1000; // in milliseconds
  const now = Date.now();
  const timeInCurrentPeriod = now % periodInterval;
  const timeRemaining = periodInterval - timeInCurrentPeriod;
  return timeRemaining;
}

let timeRemaining = $state(timeRemainingInCurrentPeriod());

onMount(() => {
  setInterval(() => {
    // update the progress bar every second
    timeRemaining = timeRemainingInCurrentPeriod();

    // also check if we need to generate a new code
    const newCode = totp.generateCode();
      if (newCode !== generatedCode){
        console.log("New code generated:", newCode);
        generatedCode = newCode;
      }
  }, 1000);
});

</script>

<div class="flex items-center justify-center h-screen w-screen bg-base-300">
  <div class="flex gap-10 items-center justify-center">
  
    <div class="mockup-phone w-72">
      <div class="mockup-phone-camera"></div>
      <div class="mockup-phone-display text-white grid place-content-center bg-neutral-900 text-center">
      <div 
        class="radial-progress" 
        style={`--value:${Math.floor(timeRemaining / 1000 / config.period * 100)}; --size:12rem; --thickness: 8px;`}
        aria-valuenow={Math.floor(timeRemaining / 1000)}
        role="progressbar"><span class="text-lg font-semibold">{generatedCode}</span></div>
      </div>
    </div>

  <fieldset class="fieldset bg-base-200 border-base-300 rounded-box w-xs border p-4">
    <legend class="fieldset-legend">Login</legend>

    <label class="label">Email</label>
    <input type="email" class="input" placeholder="Email" />

    <label class="label">Password</label>
    <input type="password" class="input" placeholder="Password" />

    <button class="btn btn-neutral mt-4">Login</button>
  </fieldset>
  </div>
</div>
