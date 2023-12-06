# Timio
## Background
Timio is an educational audio and music player for kids. It consists of a battery powered player and 20 disks with content such as nursery rhymes, games and lullabies. The content is provided in 6 languages (8 in updated versions and SD cards). English, Spanish, French, German, Dutch, Chinese, Italian, Portuguese. More info is available at [timio.co](https://timio.co/).

To change the language you need to remove the disk and hold the language selection button. This is great until you realise it's the first thing a toddler will do. So my son ends up listening to nursery rhymes in a language he has no idea about and loses interest in is new toy. I wondered if I could modify the Timio to only made some languages available.

## SD Card
Content is not actually stored on the disks. Swapping the disk just changes the content selection stored on the device. There is an SD card in the battery compartment containing the content. The instructions make it clear to not remove or modify so obviously I pulled it out and stuck it in a computer. This is what I found.

* The card provided is a Sandisk Class 4 8GB card. 
* The content is only around 170MB (>250MB on more recent SD card versions).
* Filesystem is FAT32
* One `.bin` file (`/L00/data.bin`). The firmware blob for the TIMIO's microprocessor. Blob is for a [GeneralPlus m’nSP2.0™](https://web.archive.org/web/20231205031536/https://www.generalplus.com/1LVlangLN14SVprot_noSNproduct) (pronounced as micro-n-SP 2.0) sound controller.
    * The vendor and format (spif sd/mmc serial loading) of the firmware are identified in the first 566 bytes:
      ```
      0000000  \a  \0 330 025  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
      0000010  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0  \0
      *
      0000200   G   P   -   S   P   I   F   -   H   E   A   D   E   R  -   -
      0000210   0   1   2   3   4   5   6   7   8   9   a   b   c   d  e   f
      0000220 242   a   W  \0 242   a   W  \0   G   P 005  \0  \0  \0 001  \0
      0000230 377 377  \0  \0 002   Z
      ```
* All other files on the card are `.a18` sound data files. The audio data format is a GeneralPlus "`a1800`" encoding specialized for playback on these chips.
    * `.a18` format audio can be produced via the GeneralPlus [G+ Gadget developer tools](https://web.archive.org/web/20231205035201/https://www.generalplus.com/1LVlangLNx3SVq9SNservice_n_support_d) Audio Batch Converter.

## Directory structure
At the top level there are up to 9 directories (depending on SD card version):
```
L00
L01
L02
L03
L04
L05
L06
L07
L08
```

### L00
L00 seems to contain some common files:
```
BYE.a18 - sleep-mode chime sound file
data.bin - device firmware blob
Disc.a18 - tbd
L0009S01.a18 - tbd
L0009S02.a18 - tbd
L0009S03.a18 - tbd
L0009S04.a18 - tbd
L0009S05.a18 - tbd
L0009S06.a18 - tbd
L0009S07.a18 - tbd
L0009S08.a18 - tbd
L0009S09.a18 - tbd
L0009S10.a18 - tbd
L0009S11.a18 - tbd
L0009S12.a18 - tbd
WEL.a18 - power-on chime sound
```

### L0X
The L0X directories are language specific data files. They contain a load of files named using the following standard:

```
LXXYYYYY.a18
XX      Language ID
YYYYY   Content ID
```

e.g. E:\L01\L0100C01.a18 is for language 01 (English) content 00C01.

These are the language IDs:
```
01 - English
02 - Spanish
03 - French
04 - German
05 - Dutch
06 - Chinese
07 - Italian
08 - Portuguese
```

## Content
When changing the language the, the Timio will announce the one selected. I assumed this came from the L00 directory, but after ~~breaking~~ modifying the directory structure, it seems to come from one of the LXXYYYYY.a18 files.

All languages contain the same set of content ID files named using their language ID prefix.

Every language includes 428 audio files (depending on the SD card version):
<details>
<summary>❗Click to expand complete breakdown for ALL 428 audio files per language❗</summary>
(Each language set includes its actual language id number 1-8 instead of the `*` template below)

```
L0*00C01.a18 - volume change message
L0*00C02.a18 - volume change message
L0*00C03.a18 - volume change message
L0*00Q01.a18 - question-mode on message
L0*00Q02.a18 - question-mode off message
L0*00V00.a18 - tbd
L0*00V01.a18 - tbd
L0*00V02.a18 - tbd
L0*00V03.a18 - tbd
L0*00W01.a18 - warnings?
L0*00W02.a18 - tbd
L0*00W03.a18 - tbd
L0*01S00.a18 - disc 01 (Nursery Rhymes vol. I) title message
L0*01S01.a18 - disc 01, first sound (1 o'clock position; star & moon)
L0*01S02.a18 - disc 01, second sound (2 o'clock position; doll)
L0*01S03.a18 - disc 01, third sound (3 o'clock position; wheels on the bus)
L0*01S04.a18 - etc.
L0*01S05.a18
L0*01S06.a18
L0*01S07.a18
L0*01S08.a18
L0*01S09.a18
L0*01S10.a18
L0*01S11.a18
L0*01S12.a18
L0*02S00.a18 - disc 02 (Nursery Rhymes vol. II) title message
L0*02S01.a18 - disc 02, first sound (1 o'clock position; monkey)
L0*02S02.a18 - etc.
L0*02S03.a18
L0*02S04.a18
L0*02S05.a18
L0*02S06.a18
L0*02S07.a18
L0*02S08.a18
L0*02S09.a18
L0*02S10.a18
L0*02S11.a18
L0*02S12.a18
L0*03S00.a18
L0*03S01.a18
L0*03S02.a18
L0*03S03.a18
L0*03S04.a18
L0*03S05.a18
L0*03S06.a18
L0*03S07.a18
L0*03S08.a18
L0*03S09.a18
L0*03S10.a18
L0*03S11.a18
L0*03S12.a18
L0*04S00.a18
L0*04S01.a18
L0*04S02.a18
L0*04S03.a18
L0*04S04.a18
L0*04S05.a18
L0*04S06.a18
L0*04S07.a18
L0*04S08.a18
L0*04S09.a18
L0*04S10.a18
L0*04S11.a18
L0*04S12.a18
L0*05S00.a18
L0*05S01.a18
L0*05S02.a18
L0*05S03.a18
L0*05S04.a18
L0*05S05.a18
L0*05S06.a18
L0*05S07.a18
L0*05S08.a18
L0*05S09.a18
L0*05S10.a18
L0*05S11.a18
L0*05S12.a18
L0*06S00.a18
L0*06S01.a18
L0*06S02.a18
L0*06S03.a18
L0*06S04.a18
L0*06S05.a18
L0*06S06.a18
L0*06S07.a18
L0*06S08.a18
L0*06S09.a18
L0*06S10.a18
L0*06S11.a18
L0*06S12.a18
L0*07Q01.a18 - disc 07, quiz question for 1st sound (1 o'clock position)
L0*07Q02.a18 - disc 07, quiz question for 2nd sound (2 o'clock position)
L0*07Q03.a18 - etc.
L0*07Q04.a18
L0*07Q05.a18
L0*07Q06.a18
L0*07Q07.a18
L0*07Q08.a18
L0*07Q09.a18
L0*07Q10.a18
L0*07Q11.a18
L0*07Q12.a18
L0*07S00.a18 - disc 07 title message
L0*07S01.a18 - disc 07, first sound (1 o'clock position)
L0*07S02.a18 - etc.
L0*07S03.a18
L0*07S04.a18
L0*07S05.a18
L0*07S06.a18
L0*07S07.a18
L0*07S08.a18
L0*07S09.a18
L0*07S10.a18
L0*07S11.a18
L0*07S12.a18
L0*08Q01.a18
L0*08Q02.a18
L0*08Q03.a18
L0*08Q04.a18
L0*08Q05.a18
L0*08Q06.a18
L0*08Q07.a18
L0*08Q08.a18
L0*08Q09.a18
L0*08Q10.a18
L0*08Q11.a18
L0*08Q12.a18
L0*08S00.a18
L0*08S01.a18
L0*08S02.a18
L0*08S03.a18
L0*08S04.a18
L0*08S05.a18
L0*08S06.a18
L0*08S07.a18
L0*08S08.a18
L0*08S09.a18
L0*08S10.a18
L0*08S11.a18
L0*08S12.a18
L0*09Q01.a18
L0*09Q02.a18
L0*09Q03.a18
L0*09Q04.a18
L0*09Q05.a18
L0*09Q06.a18
L0*09Q07.a18
L0*09Q08.a18
L0*09Q09.a18
L0*09Q10.a18
L0*09Q11.a18
L0*09Q12.a18
L0*09S00.a18
L0*09S01.a18
L0*09S02.a18
L0*09S03.a18
L0*09S04.a18
L0*09S05.a18
L0*09S06.a18
L0*09S07.a18
L0*09S08.a18
L0*09S09.a18
L0*09S10.a18
L0*09S11.a18
L0*09S12.a18
L0*10Q01.a18
L0*10Q02.a18
L0*10Q03.a18
L0*10Q04.a18
L0*10Q05.a18
L0*10Q06.a18
L0*10Q07.a18
L0*10Q08.a18
L0*10Q09.a18
L0*10Q10.a18
L0*10Q11.a18
L0*10Q12.a18
L0*10S00.a18
L0*10S01.a18
L0*10S02.a18
L0*10S03.a18
L0*10S04.a18
L0*10S05.a18
L0*10S06.a18
L0*10S07.a18
L0*10S08.a18
L0*10S09.a18
L0*10S10.a18
L0*10S11.a18
L0*10S12.a18
L0*11Q01.a18
L0*11Q02.a18
L0*11Q03.a18
L0*11Q04.a18
L0*11Q05.a18
L0*11Q06.a18
L0*11Q07.a18
L0*11Q08.a18
L0*11Q09.a18
L0*11Q10.a18
L0*11Q11.a18
L0*11Q12.a18
L0*11S00.a18
L0*11S01.a18
L0*11S02.a18
L0*11S03.a18
L0*11S04.a18
L0*11S05.a18
L0*11S06.a18
L0*11S07.a18
L0*11S08.a18
L0*11S09.a18
L0*11S10.a18
L0*11S11.a18
L0*11S12.a18
L0*12Q01.a18
L0*12Q02.a18
L0*12Q03.a18
L0*12Q04.a18
L0*12Q05.a18
L0*12Q06.a18
L0*12Q07.a18
L0*12Q08.a18
L0*12Q09.a18
L0*12Q10.a18
L0*12Q11.a18
L0*12Q12.a18
L0*12S00.a18
L0*12S01.a18
L0*12S02.a18
L0*12S03.a18
L0*12S04.a18
L0*12S05.a18
L0*12S06.a18
L0*12S07.a18
L0*12S08.a18
L0*12S09.a18
L0*12S10.a18
L0*12S11.a18
L0*12S12.a18
L0*13Q01.a18
L0*13Q02.a18
L0*13Q03.a18
L0*13Q04.a18
L0*13Q05.a18
L0*13Q06.a18
L0*13Q07.a18
L0*13Q08.a18
L0*13Q09.a18
L0*13Q10.a18
L0*13Q11.a18
L0*13Q12.a18
L0*13S00.a18
L0*13S01.a18
L0*13S02.a18
L0*13S03.a18
L0*13S04.a18
L0*13S05.a18
L0*13S06.a18
L0*13S07.a18
L0*13S08.a18
L0*13S09.a18
L0*13S10.a18
L0*13S11.a18
L0*13S12.a18
L0*14Q01.a18
L0*14Q02.a18
L0*14Q03.a18
L0*14Q04.a18
L0*14Q05.a18
L0*14Q06.a18
L0*14Q07.a18
L0*14Q08.a18
L0*14Q09.a18
L0*14Q10.a18
L0*14Q11.a18
L0*14Q12.a18
L0*14S00.a18
L0*14S01.a18
L0*14S02.a18
L0*14S03.a18
L0*14S04.a18
L0*14S05.a18
L0*14S06.a18
L0*14S07.a18
L0*14S08.a18
L0*14S09.a18
L0*14S10.a18
L0*14S11.a18
L0*14S12.a18
L0*15Q01.a18
L0*15Q02.a18
L0*15Q03.a18
L0*15Q04.a18
L0*15Q05.a18
L0*15Q06.a18
L0*15Q07.a18
L0*15Q08.a18
L0*15Q09.a18
L0*15Q10.a18
L0*15Q11.a18
L0*15Q12.a18
L0*15S00.a18
L0*15S01.a18
L0*15S02.a18
L0*15S03.a18
L0*15S04.a18
L0*15S05.a18
L0*15S06.a18
L0*15S07.a18
L0*15S08.a18
L0*15S09.a18
L0*15S10.a18
L0*15S11.a18
L0*15S12.a18
L0*16Q01.a18
L0*16Q02.a18
L0*16Q03.a18
L0*16Q04.a18
L0*16Q05.a18
L0*16Q06.a18
L0*16Q07.a18
L0*16Q08.a18
L0*16Q09.a18
L0*16Q10.a18
L0*16Q11.a18
L0*16Q12.a18
L0*16S00.a18
L0*16S01.a18
L0*16S02.a18
L0*16S03.a18
L0*16S04.a18
L0*16S05.a18
L0*16S06.a18
L0*16S07.a18
L0*16S08.a18
L0*16S09.a18
L0*16S10.a18
L0*16S11.a18
L0*16S12.a18
L0*17Q01.a18
L0*17Q02.a18
L0*17Q03.a18
L0*17Q04.a18
L0*17Q05.a18
L0*17Q06.a18
L0*17Q07.a18
L0*17Q08.a18
L0*17Q09.a18
L0*17Q10.a18
L0*17Q11.a18
L0*17Q12.a18
L0*17S00.a18
L0*17S01.a18
L0*17S02.a18
L0*17S03.a18
L0*17S04.a18
L0*17S05.a18
L0*17S06.a18
L0*17S07.a18
L0*17S08.a18
L0*17S09.a18
L0*17S10.a18
L0*17S11.a18
L0*17S12.a18
L0*18Q01.a18
L0*18Q02.a18
L0*18Q03.a18
L0*18Q04.a18
L0*18Q05.a18
L0*18Q06.a18
L0*18Q07.a18
L0*18Q08.a18
L0*18Q09.a18
L0*18Q10.a18
L0*18Q11.a18
L0*18Q12.a18
L0*18S00.a18
L0*18S01.a18
L0*18S02.a18
L0*18S03.a18
L0*18S04.a18
L0*18S05.a18
L0*18S06.a18
L0*18S07.a18
L0*18S08.a18
L0*18S09.a18
L0*18S10.a18
L0*18S11.a18
L0*18S12.a18
L0*19Q01.a18
L0*19Q02.a18
L0*19Q03.a18
L0*19Q04.a18
L0*19Q05.a18
L0*19Q06.a18
L0*19Q07.a18
L0*19Q08.a18
L0*19Q09.a18
L0*19Q10.a18
L0*19Q11.a18
L0*19Q12.a18
L0*19S00.a18
L0*19S01.a18
L0*19S02.a18
L0*19S03.a18
L0*19S04.a18
L0*19S05.a18
L0*19S06.a18
L0*19S07.a18
L0*19S08.a18
L0*19S09.a18
L0*19S10.a18
L0*19S11.a18
L0*19S12.a18
L0*20S00.a18
L0*20S01.a18
L0*20S02.a18
L0*20S03.a18
L0*20S04.a18
L0*20S05.a18
L0*20S06.a18
L0*20S07.a18
L0*20S08.a18
L0*20S09.a18
L0*20S10.a18
L0*20S11.a18
L0*20S12.a18
```
</details>

## Modifying Languages
My aim was to remove the languages that I don't want to use that the moment so that's where I started.

*Take a backup before doing anything and do not modify the included card* I doubt the manufacturer will be happy when you ask for a replacement.
I backed up my files and used a new SD card formatted for FAT32 for testing. I initially copied all the files over and tested it still worked in the Timio.

## Removing Languages
Since the file naming has been identified, we can replace the languages we don't want, with one we do. In these cases I kept the L00 directory untouched.

### To replace all with English
With SD mounted to E:\ and backup of the Timio files in C:\temp\timio
```
robocopy e: C:\temp\timio /E
```
Replace all languages with English files
```powershell
foreach ($id in (2..6)) {
    mkdir "e:\L0$id\";
    Get-ChildItem C:\temp\timio\L01\ | %{
        cp  $_.fullname  "e:\L0$id\$($_.name -replace '^L01', "L0$id")"
    }
}
```
When changing language now the Timio will only use English.

### To replace with only English and Spanish
In this case I replace alternate languages with English and Spanish. This way cycling through the languages with switch between the 2.
```powershell
foreach ($id in (1, 3, 5, 7)) {
    mkdir "e:\L0$id\";
    Get-ChildItem C:\temp\timio\L01\ | %{
        cp  $_.fullname "e:\L0$id\$($_.name -replace '^L01', "L0$id")"
    }
}

foreach ($id in (2, 4, 6, 8)) {
    mkdir "e:\L0$id\";
    Get-ChildItem C:\temp\timio\L02\ | %{
        cp  $_.fullname "e:\L0$id\$($_.name -replace '^L02', "L0$id")"
    }
}
```

## Next Steps
* [x] ~~I have no idea what format the .a18 are. I'd like to find this out and see if content can be modified.~~
    * [X] TIMIO will indeed play a different sound, when a new `.a18` is copied into a `L0X/` directory and renamed to replace an existing file (e.g.: making the Tamborine icon on Instruments disc play the Tuba sound, making an entire language play the same sound for all button presses by replacing the data content of all files in an `LOX/` directory). 👍
    * [ ] Verify that new, custom sounds be encoded as `.a18`  using GeneralPlus G+ Gadget tools.
* [ ] I'd like work out how the content ID is linked to the disks. 
* [ ] Can L0X directories be removed or added?
