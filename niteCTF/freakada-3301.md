# Freakada 3301

The most fun out of all challenges.

1. ran `exiftool` on the image to extract the message, decrypted it using dcode's mono-alphabetic substitution cipher decryptor tool.

2. visited the link mentioned in the unenecrypted ciphertext, downloaded the duck image from the proton drive link.

3. on putting the image in `aperisolver`, I could see a link in the section which runs `strings` on the image.

4. the link took me to a github repo, on seeing the commits I could see a poem and multiple lines of numbers, I remembered the original cicada-3301 challenge (thanks to Lemmino's video on it) and used the numbers `x:y:z` which corresponded to x'th line's y'th word's z'th character, it all came together to form a discord link

5. I joined the server and joined the VC, after listening to Thick of It, I saw an app on the rightside in the members column on discord which had the description `give me my prime, i'll give you my location`

6. I entered 3301 first after which it said:-

```

multiply 3 certain primes, 3301 * x * y.
Where, x and y are integral part of the original image.

```

Once again I remembered the Original cicada-3301 challenge which involved multiplying 3301 and multiplying two primes which denoted the dimensions of the original image. My teammate entered `745520947` which was the product and got the co-ordinates which corresponded to `Freaky Backshots Hill` in Manipal


7. The images on google maps had a QR code, on scanning that QR code I got another proton drive link with an 
audio file in it

8. On opening the, audio file in audacity and viewing it's spectrograph I saw the first part of the flag

9. I was confused for quite a while on how to decrypt the second part of the flag which I got from the github 
repo README earlier but when retracing my steps I noticed dots under the unencrypted part of the flag in the 
spectrograph

10. On decrypting the morse code, I got the word `FREEKEY` which I used as a key for Vignere Cipher and decrypted the second part as well.

flag: `nite{4_wannabe_1nt3rn3t_my5tery}`

