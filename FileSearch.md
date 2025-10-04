# File Search, Analysis & Archiving in Linux

I started by unzipping and untarring the file with `bunzip2` and `tar`
```
hazoor@ICT171Labs:~/Downloads$ ls -l
total 4
-rw-rw-r-- 1 hazoor hazoor 399 Oct  2 19:55 Gutenberg.tar.bz2
hazoor@ICT171Labs:~/Downloads$ bunzip2 Gutenberg.tar.bz2
hazoor@ICT171Labs:~/Downloads$ tar xvf Gutenberg.tar 
moby.txt
twocities.txt
frankenstein.txt
hazoor@ICT171Labs:~/Downloads$ ls -l
total 24
-rw-r--r-- 1 hazoor hazoor    94 Jul  7 15:18 frankenstein.txt
-rw-rw-r-- 1 hazoor hazoor 10240 Oct  2 19:55 Gutenberg.tar
-rw-r--r-- 1 hazoor hazoor    16 Jul  7 15:18 moby.txt
-rw-r--r-- 1 hazoor hazoor    52 Jul  7 15:18 twocities.txt
```

I pulled a few files off of project gutenberg to try some things. I started by looking for a single word within the files.
```
hazoor@ICT171Labs:~/Downloads$ grep -r "verdigris"
pg35.txt:with verdigris. It chanced that the face was towards me; the sightless
pg35.txt:a coil in the decorations, and the verdigris came off in powdery
```
I found that it printed the whole line that the given word was in.

I then looked for a phrase, using the `-C` switch to get some lines of context
```
hazoor@ICT171Labs:~/Downloads$ grep -r -C 3 "Next day there was a surprise for Jack"
1213-0.txt-of the forty thousand in the sack--a hundred and thirty-three thousand
1213-0.txt-altogether.
1213-0.txt-
1213-0.txt:Next day there was a surprise for Jack Halliday. He noticed that
1213-0.txt-the faces of the nineteen chief citizens and their wives bore that
1213-0.txt-expression of peaceful and holy happiness again. He could not
1213-0.txt-understand it, neither was he able to invent any remarks about it
```

I moved over to my documents directory to see if I could find the age of files within
```
hazoor@ICT171Labs:~/Documents$ find -printf '%T+ %p\n'
2025-10-01+17:40:41.4196256690 .
2025-09-30+11:17:03.8800245150 ./Hello.odt
2025-10-01+17:37:10.0000000000 ./books.tar
2025-10-01+17:35:41.0000000000 ./books
2025-07-03+01:09:36.0000000000 ./books/12-0.txt
2025-06-15+01:09:12.0000000000 ./books/76-0.txt
2021-11-27+15:25:12.0000000000 ./books/36-0.txt
2025-09-30+11:48:44.4209562020 ./testfile2.txt
```
I found the books I downloaded from Project Gutenberg to be the oldest files, with 12-0.txt (Through The Looking Glass)

I looked through my filesystem to find files exceeding 10 Mebibytes in size in my home folder
```
hazoor@ICT171Labs:~$ find . -type f -size +10M -exec ls -lh {} \;
-rw-r--r-- 1 hazoor hazoor 11M Sep 30 12:04 ./snap/firefox/common/.cache/mozilla/firefox/ujtb2nl4.default/startupCache/scriptCache-current.bin
-rw-r--r-- 1 hazoor hazoor 17M Oct  1 16:58 ./snap/firefox/common/.mozilla/firefox/ujtb2nl4.default/storage/permanent/chrome/idb/3870112724rsegmnoittet-es.sqlite
-rw-r--r-- 1 hazoor hazoor 11M Sep 27 15:34 ./.local/share/Trash/files/books.tar
-rw------- 1 hazoor hazoor 17M Sep 30 12:24 './.config/opera/Safe Browsing/UrlSoceng.store.4_13403679874319838'
```

I looked back in my Downloads folder to test out the `du` command
```
hazoor@ICT171Labs:~/Downloads$ du -a
4	./twocities.txt
4	./moby.txt
200	./pg35.txt
12	./Gutenberg.tar
4	./frankenstein.txt
104	./1213-0.txt
332	.
```
As per the documentation, this command estimates file space usage. By default, it lists the current directory's file space in kibibytes. With the `-a` switch, this listed all the files in the given directory as well and provided their estimated file space usage.

I expermented with `sed` on a file
```
hazoor@ICT171Labs:~/Downloads$ sed -e 's/\s/\n/g' < moby.txt 
Call
me
Ishmael.hazoor@ICT171Labs:~/Downloads$ 
```
The output was a little awkward, though expected as there was no whitespace at the end. This `sed` command replaced all instances of whitespace with a new line. This did not alter the original file.

I used `grep` to find the author of one of the books I downloaded
```
hazoor@ICT171Labs:~/Downloads$ grep -i "author" pg1107.txt 
Author: William Shakespeare
```

## Reflections

- Which command-line tool was the most useful in solving the questions?
    - `grep` was the quickest and easiest to use in terms of searching through files, along with having the simplest usage.
- How might these search tools help in cybersecurity investigations?
    - These tools can be very useful in processing logs and looking for specigic files.
- How could scripting improve repetitive search tasks?
    - Yes. Scripting can automate processing of files.
- What limitations did you encounter using grep and find?
    - Main limitations are my own understanding, which some time with the manual pages would be able to solve. I have only scratched the surface of what these commands are capable of.
