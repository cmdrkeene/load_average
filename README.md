# Load Average

This implements UNIX-style load average computation.

For a more in depth discussion, see [this article](http://www.linuxjournal.com/article/9001?page=0,1).

Yes, there are global variables. This was designed as a system-wide calculator
but could easily be modified to track multiple types of events.

@nakajima points out that it's constant time. And that's a good thing.

# Usage

There are two major operations: `tick` and `compute`.

When whatever you want to measure (e.g. requests, jobs, etc) happens, call 
`tick`.

On some interval, you need to call `compute`. You must pass that interval to 
`compute`.

## Example

    require 'rubygems'
    require 'eventmachine'
    require 'load_average'

    INTERVAL = 5 #seconds
    
    EventMachine.run do
      EventMachine::add_periodic_timer(INTERVAL) do
        LoadAverage.compute(INTERVAL)
        puts $load.inspect
      end
    
      # Simulate events
      EventMachine::add_periodic_timer(0.25) do
        (10000.0 * rand).to_i.times do
          LoadAverage.tick
        end
      end
    end

## Credits

Some UNIX guys?