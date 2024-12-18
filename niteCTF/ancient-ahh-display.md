# Ancient Ahh Display

_I didn't submit the flag for this one but I was the one who got it and since I spent enough time on this I'm writing this_

A pretty unique problem, I had initially no idea how to approach this but I got the flag with a teammates knowledge who explained it to me.

Basically we were to select columns from the excel file which gave us 5 bit binaries which were mapped to 7 bit binaries which can be displayed on a seven segment LED screen.

some important facts to note were:-

1. The 5 bit binary numbers were in little-endian and were to be reversed before mapping to 7-bit binaries.

2. The 7 bit binary numbers were also in little-endian and we also needed to invert every bit as the number represented LED turning OFF from an ON state.

After taking care of these 2 facts and correctly mapping everything, I looked up a seven segment display and 
drew every character by hand.

flag: `nite{tr0UbLe_bUbbLe_L0L}`

