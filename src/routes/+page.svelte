<script lang="ts">
import {Totp, OtpAlgorithm} from "@devolutions/slauth";

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
console.log(totp.generateCode());
</script>

<div class="flex items-center justify-center h-screen bg-base-300">
  <div class="card card-side bg-base-100 shadow-sm">
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
