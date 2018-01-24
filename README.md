# shellplot

## Synopsis

This script reads csv lines from stdin and creates a plot which it displays inline 
in your shell window if you are using the [**mlterm**](http://mlterm.sourceforge.net) terminal emulator program. It
does this by using pandas and matplotlib to process the csv data and generate an
image of a plot, which it converts to 'sixel' format and writes to stdout. The 
mlterm terminal emulator recognizes the 'sixel' stream and converts it into pixels
which it draws inline with your shell output.

## Examples
*(screenshots of mlterm running bash commands)*

```
sar -u | grep all | sed -e 's/all//' -e 's/   */, /g' | grep -v Ave | shellplot -stacked -area -labels "user nice system iowait steal idle" -map flag
```
![sar cpu screenshot](docs/images/sar_cpu.png?raw=true "sar CPU")


```
sar -r | grep -v used | grep -v Ave | awk '{print $1, $2, ", ", $5}' |grep -v Linux | grep -v '  ,' | shellplot -area -label0 'Memory used'
```
![sar memory screenshot](docs/images/sar_mem.png?raw=true "sar Memory")


```
grep -h UPLOAD vsftpd.log* |  sed -e 's/\[pid.* \([0-9]*\) bytes.*/, \1/' | shellplot -area -title "FTP bytes received by time"
```
![ftp bytes screenshot](docs/images/vsftpd_log.png?raw=true "vsftpd bytes")


```
aws ec2 describe-spot-price-history --availability-zone us-west-2c --instance-types r3.4xlarge --product-description "Linux/UNIX (Amazon VPC)"  --output text | awk '{print $6, ", ", $5}' | shellplot
```
![AWS Spot prices](docs/images/aws_spot_price_history.png?raw=true "AWS Spot prices")


```
grep 'total elapsed time' 2018*log | sed -e 's/.log.*total elapsed time:/,/' -e 's/minutes.*//' | plot -bar -label0 'Log file'
```
![categorical plots](docs/images/categorical.png?raw=true "Categorical plot")

## Motivation

The [sixelplot project](https://github.com/kktk-KO/sixelplot) has a hint of this idea but it doesn't actually implement the features of pandas and matplotlib to automagically create plots by making guesses about the data.

## Installation

+ Download the shellplot script and place it in your path. 
+ Download the source to libsixel, compile and install it so that it puts the utility img2sixel in your path.
+ Use the mlterm terminal emulator (instead of xterm, putty, konsole, gnome-terminal, etc.)

DEPENDENCIES:

+ mlterm - http://mlterm.sourceforge.net
+ img2sixel - https://github.com/saitoha/libsixel

and the following python packages

+ pandas
+ matplotlib
+ dateutil
+ textwrap
+ numpy

## API Reference

OPTIONS:

    -maps (use by itself to show what all the colormaps look like so you can pick one you like)
    -map "colormap name" (pick a map name displayed by the -maps option; defaults to colormap 'tab20')
    -title "Some text" (defaults to displaying the value of environment variable FULL_COMMAND_LINE or BASH_COMMAND)
    -output "path.png" (save a copy of the plot to a png file)
    -labels "something1 something2 something3..." (whitespace separated column labels)
    -label1 "something" (specific label for column 1)
    -labelN "something" (specific label for column N)
    -line (draw a line plot)
    -bar (draw a bar plot)
    -area (draw an area plot)
    -stacked (stack multiple columns if there are more than one)

## Tests

Run some of the sample commands above and compare your screen to the screenshots.

## Contributors

Mr. Average

## License

MIT
