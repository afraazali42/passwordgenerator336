# Password Generator 336

**Live at [passwordgenerator336.com](https://passwordgenerator336.com)**

## How it works

The whole point of this tool is that you can check these claims yourself. The
generator is a few hundred lines of plain JavaScript in `index.html`, not
minified and with no build step, so the source you read is exactly the code that
runs.

### Where the randomness comes from

Every random choice is drawn from
[`crypto.getRandomValues()`](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/getRandomValues),
the browser's cryptographically secure random number generator (CSPRNG). It is
seeded by the operating system from hardware entropy and belongs to the same
class of randomness used to generate TLS keys. The code never touches
`Math.random()`, which is fast but predictable and unfit for secrets.

There is no server in the loop. Some password sites generate the password on a
server and send it to your browser over an encrypted connection; this one builds
everything on your own machine, so a finished password never crosses a network or
rests on anyone else's computer.

### Turning random numbers into characters, without bias

Picking a uniformly random character from a pool of N options hides a subtle
trap. The obvious approach, `randomNumber % N`, skews the odds whenever N does
not divide the random range evenly, leaving some characters slightly more likely
than others. This is known as modulo bias.

`secureRandomInt(max)` sidesteps it with rejection sampling: it finds the largest
multiple of `max` that fits in a 32-bit range, draws a random 32-bit number, and
if that number falls in the small leftover region above the cutoff, it discards
it and draws again. Only numbers from the evenly divisible part are accepted, so
every character ends up exactly equally likely. (This makes it a "Las Vegas"
algorithm: always correct, with a running time that now and then takes an extra
draw.)

### Building a password that obeys your rules

Each character is drawn from the combined pool of the sets you have switched on
(uppercase, lowercase, numbers, symbols), minus anything in the exclude list. The
optional rules are enforced while the password is assembled: a candidate is
rejected and redrawn if it would repeat an earlier character (no-duplicates), land
right beside the previous character in code order such as `ab` or `34`
(no-sequential), or open the password as a non-letter (start-with-letter). If a
configuration is impossible, for instance no-duplicates with a pool smaller than
the requested length, it stops cleanly rather than looping forever. A final pass
retries a handful of times so that every set you enabled actually shows up when
there is room for it.

### Honest entropy

The "Entropy: X bits" readout is calculated, not decorative. The naive estimate
is `length * log2(poolSize)`, but the constraints shrink the real keyspace, so
that figure would overstate things. Start-with-letter draws the first slot from a
smaller, letters-only pool; no-duplicates gives each later slot one fewer option
than the last (sampling without replacement). The readout instead sums
`log2(choices)` across every position, so the bit count reflects the keyspace an
attacker would really face.

## License: [GPL-3.0](LICENSE)
