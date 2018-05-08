Batch Check-in Label Cutting for RockRMS
========================================

This mod/plugin allows cutting checkin lables in batches - this means all of a family's labels print out at the same time instead of cutting off each individual label. Note: If you design your labels right, you might not need to use this!

## ZPL Explanation

ZPL Reference Guide: https://www.zebra.com/content/dam/zebra/manuals/en-us/software/zpl-zbi2-pm-en.pdf

There are a few zpl commands we determined to be important for continuous/batch printing. This mod automatically removes/inserts them when appropriate (using Method 1 below), but if you have a fixed set of labels for an environment you could manually tweak the labels to work without needing this mod.


`^MM` - Print Mode
 - Changes the print mode
 - Common examples:
    - `^MMT` - Tear-off Mode
    - `^MMP` - Peal-off Mode
    - `^MMC` - Cutter Mode
    - `^MMD` - Delayed Cutter Mode

`^PQ` - Print Quantity
 - Specifies how many labels to print at a time
 - Example: `^PQ1,1,1,Y`

`^XB` - Prevent Backfeed
 - Prevents cutting
 - Prevents backfeeding

`~JK` - Cut (In Delayed Mode)
 - Activates cutting when in Delayed Cut mode
 - Needs to be sent as a seperate label

## Method 1: Cutter Mode + Prevent Cutting

This is the method we use for this mod - it uses a command to prevent cutting until the last label.

1. Make sure your printer is in Cutter Mode.
2. Remove any `^MM*` commands from all labels so the printer doesn't change modes while printing.
3. Remove any `^PQ1,1,1,Y` commands from all labels, since this would cause them to be batched in groups of 1.
4. Add a `^XB` command to all labels *except* the last one - this will delay cutting and backfeeding untill the last label is printed.

## Method 2: Delayed Cutter Mode + Cut (Untested)

This uses the built-in Delayed Cut mode on the printer.

1. Make sure your printer is in Delayed Cutter Mode.
2. Remove any `^MM*` commands from all labels so the printer doesn't change modes while printing.
3. Remove any `^PQ1,1,1,Y` commands from all labels, since this would cause them to be batched in groups of 1.
4. Create a new "Cut" label with only a `~JK` command and add it as the last label to all of your label configurations (or wherever you want it to cut).

If you have issues with backfeeding you can probably add a `^XB` command to all of your labels (except your "Cut" label of course).

## Method 3: Switching modes while printing (Untested)

This one is the simplist, but also feels a bit hacky - it just switches the printing mode for each label to control when to cut. It will most likely have issues with backfeeding, but that hasn't been tested.

1. Remove any `^PQ1,1,1,Y` commands from all labels, since this would cause them to be batched in groups of 1.
2. Add a `^MMC` command to any label you want to get cut
3. Add a `^MMT` command to any label you don't want to get cut

If you have issues with backfeeding you can probably add a `^XB` command to all of your `^MMT` labels.

## How to Install

Warning: this will restart your Rock server, so do it sometime when it's not getting used! 

1. Download the latest `.zip` file from https://github.com/NewPointe/rock-plugin-checkinbatchcut/releases
2. Extract and copy the files into the root of your RockWeb folder (merging folders if needed). For most people, this will be `C:\inetpub\wwwroot`.
3. Follow the rest of the instructions starting at step 4 here: http://shouldertheboulder.com/Article?id=497