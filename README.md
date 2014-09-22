Make an Automator Service 
/Application/Automator -> Service -> Run AppleScript

Copy and paste the code to AppleScript content 

    # the script written by ptcong90
    on run {input}
    	set savePath to "/Users/ptcong/Desktop"
    	set fileType to "png"
    	set fileName to "Screen Shot"
    	set keepRetinaScreenShot to "NO"
    	
    	set fileName to fileName & " " & get_time()
    	set fileName1X to savePath & "/" & fileName & "@1x." & fileType
    	set fileName2X to savePath & "/" & fileName & "@2x." & fileType
    	
    	do shell script "screencapture -i -r -t " & fileType & " \"" & fileName2X & "\""
    	
    	tell application "System Events"
    		if exists file fileName2X then
    			tell application "Image Events"
    				set theImage to open fileName2X
    				scale theImage by factor 0.5
    				save theImage in fileName1X
    				close theImage
    			end tell
    			if keepRetinaScreenShot is not equal to "YES" then
    				do shell script "rm \"" & fileName2X & "\""
    			end if
    		end if
    	end tell
    end run
    
    on get_time()
    	set now to current date
    	set sHours to hours of now
    	if sHours is greater than 12 then
    		set sHours to sHours - 12
    	end if
    	set sTime to time string of now
    	return (year of now) & "-" & zero_pad(month of now as number, 2) & "-" & zero_pad(day of now as number, 2) & " at " & sHours & "." & zero_pad(minutes of now, 2) & "." & zero_pad(seconds of now, 2) & " " & text ((length of sTime) - 1) thru length of sTime
    end get_time
    
    on zero_pad(value, sLength)
    	set sResult to value
    	set repeatTimes to sLength - (length of (value as string))
    	if repeatTimes is greater than 0 then
    		repeat repeatTimes times
    			set sResult to "0" & sResult as string
    		end repeat
    	end if
    	return sResult
    end zero_pad

Save with name "Make Screenshot.workflow"

Go To System Preferences -> Keyboard -> Shortcut -> Services -> General -> Add shortcut "Cmd+Shift+5" for "Make Screenshot".
Now you can use Cmd+Shift+5 to capture screenshot for non-retina solution.

There are some configurations that you may change.

`set savePath to "/Users/ptcong/Desktop"`, change "ptcong" to your account

`set fileType to "png"` 

`set fileName to "Screen Shot"`

`set keepRetinaScreenShot to "NO"` "YES" or "NO"

