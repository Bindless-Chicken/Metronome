<h1 align=center>
   <img src="https://user-images.githubusercontent.com/1039134/36032726-379708c8-0daf-11e8-9afc-6ba6e89d542a.jpg" style="max-width: 100%; min-height: 300px;" original-title="">
    <br>
    Metronome
    <br>
    <br>
    <a href="https://oshpark.com/shared_projects/o1i7L0Sa"><img src="https://oshpark.com/assets/badge-5b7ec47045b78aef6eb9d83b3bac6b1920de805e9a0c227658eac6e19a045b9c.png" alt="Order from OSH Park"></img></a>
</h1>

## Theory of operation
The board is designed only around basic logic chips, hence why you won't find any microcontrollers here. The circuit can be split into two sub-circuits, the tune generation, and the pulse generation.

### Tune generation
To act as a proper metronome, we needed to have two different tune: one for the first pulse and a second for the other three. Tune generation is done using a NE555 *U1* which output is fed into a 74HC163 *U3* counter. The counter in that case is acting as a frequency divider, dividing the 2kHz output of the astable 555 timer into a 1kHz signal and a 500Hz one.

Effectively, at the end of the tune generator we are left with to square waves at 1kHz and 500Hz, which are respectively a C5 and a C6.

### Pulse generation
Once again a NE555 *U2* astable timer is used to generate the pulsation, using a potentiometer the output frequency can be changed between 0.53Hz and 3.47Hz, effectively changing the tempo between 40 BPM and 208 BPM.

The output of the timer is fed into a CD4017 *U4* Johnson counter. The output of the counter are the four outputs P1, P2, P3, and P4 triggering in order, one after another.

A shorter pulse is generated in order to have a more punchy effect on the pulse. This is done by reducing the original pulse duration using a RC delay line *U7*.

### Putting it all together
For silent operation, four LEDs have been put at the top, they are directly wired to the Johnson counter outputs.

To get the right sound at the right time, the tunes generated previously are `and` *U6* with their respective pulse; high pitch with the first output and low pitch with the others. For optimization sake, the other three are `and` *U6* with the `not` *U5* of the first pulse. Finally, all the signaled are put together using an `or` *U8*.

## BOM

| Reference      |  Value          |  Footprint                                        |
|----------------|-----------------|---------------------------------------------------|
| U1, U2         | NE555           | SOIC-8 3.9x4.9mm Pitch 1.27mm                     |
| U3             | 74HC16          | SOIC-16_3.9x9.9mm Pitch 1.27mm                    |
| U4             | CD4017          | SOIC-16 3.9x9.9mm Pitch 1.27mm                    |
| U5             | MC74VHC1GU04    | SOT-23-5                                          |
| U6             | 74LS08          | SOIC-14 3.9x8.7mm Pitch 1.27mm                    |
| U7             | SN74LVC1G08     | SOT-23-5                                          |
| U8             | SN74LVC1G32     | SOT-23-5                                          |
| R1             | 330             | 0805                                              |
| R2, R3, R4     | 91              | 0805                                              |
| R5, R6, R7     | 5.1k            | 0805                                              |
| R8             | 12k             | 0805                                              |
| R9             | 10k             | 0805                                              |
| C1             | 47nF            | 0805                                              |
| C2             | 10uF            | 0805                                              |
| C3, C4, C5, C6 | 10nF            | 0805                                              |
| C7             | 1uF             | 0805                                              |
| D1             | Red             | LED D5.0mm                                        |
| D2, D3, D4     | Green           | LED D5.0mm                                        |
| RV1            | 100k            | Trimmer Vishay TS53YL                             |
| P1             | USB_OTG         | 10118193-0001LF                                   |