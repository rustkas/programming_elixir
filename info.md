```code
r(DoParallel)
result = DoParallel.pmap 1..1000, &(&1 * &1)

.iex.exs
IEx.configure colors: [ eval_result: [ :cyan, :bright ] ]

mix new ex2-hello --app hello && cd ex2-hello && iex -S mix

elixir hello.exs

a = [1, 2, 3]
a = 4
4 = a
[a, b] = [ 1, 2, 3 ]
a = [ [ 1, 2, 3 ] ]
[a] = [ [ 1, 2, 3 ] ]
[[a]] = [ [ 1, 2, 3 ] ]

[a, b] = [ 1, 2, 3 ]
a = [ [ 1, 2, 3 ] ]
[ a..5 ] = [ 1..5 ]
[a] = [ [ 1, 2, 3 ] ]
[[a]] = [ [ 1, 2, 3 ] ]

[1, _, _] = [1, 2, 3]

[1, _, _] = [1, "cat", "dog"]

[a, a] = [1, 1]

[b, b] = [1, 2]

[b, a] = [1, 2]
^a=2 #pin previous value "a". The value "a" will not be changed
[^a, 2, 3 ] = [ 1, 2, 3 ]

[ a, b, a ] = [ 1, 2, 3 ]
[ a, b, a ] = [ 1, 1, 2 ]
[ a, b, a ] = [ 1, 2, 1 ]



Regex.run ~r{[aeiou]}, "caterpillar"
Regex.scan ~r{[aeiou]}, "caterpillar"
Regex.split ~r{[aeiou]}, "caterpillar"
Regex.replace ~r{[aeiou]}, "caterpillar", "*"

{status, file} = File.open("mix.exs")
[ 1, 2, 3 ] ++ [ 4, 5, 6 ]
iex> [ 1, 2, 3 ] ++ [ 4, 5, 6 ] # concatenation
[1, 2, 3, 4, 5, 6]
iex> [1, 2, 3, 4] -- [2, 4] # difference
[1, 3]
iex> 1 in [1,2,3,4] # membership
true
iex> "wombat" in [1, 2, 3, 4]
false

pair_tuple_to_list = fn {a,b} -> [a,b] end
pair_tuple_to_list.({1234, 5678})
handle_open = fn
{:ok, file} -> "Read data: #{IO.read(file, :line)}"
{_, error} -> "Error: #{:file.format_error(error)}"
end
handle_open.(File.open("code/intro/hello.exs"))
handle_open.(File.open("pytania.txt"))

handle_open = fn 
 
{:ok, file}  -> "First line: #{IO.read(file, :line)}"
 
{_,   error} -> "Error:  #{:file.format_error(error)}"

end

fizz_word = fn
0, 0, _ -> "FizzBuzz"
0, _, _ -> "Fizz"
_, 0, _ -> "Buzz"
_, _, n -> n
end

fizz_word = fn
0, 0, _ -> "FizzBuzz"
0, _, _ -> "Fizz"
_, 0, _ -> "Buzz"
_, _, n -> n
end
fizz_word(0,1,1)
fizz_word(0,1,1)


fb = fn n ->
fizz_word.(rem(n, 3), rem(n, 5), n)
end

mrs = prefix.("Mrs")
mrs.("Smith")
prefix.("Elixir").("Rocks")
prefix = fn string1 -> (fn word -> string1 <> " " <> word end) end

Enum.map [1,2,3,4], fn x -> x + 2 end
Enum.map [1,2,3,4], &(&1+2)
Enum.each [1,2,3,4], &(IO.inspect &1)


defmodule Chop do

def guess(actual, range = low..high) do
guess = div(low+high, 2)
IO.puts "Is it #{guess}?"
_guess(actual, guess, range)
end

defp _guess(actual, actual, _), do: IO.puts "Yes, it's #{actual}"

defp _guess(actual, guess,  _low..high) when guess < actual, do: guess(actual, guess+1..high)

defp _guess(actual, guess,  low.._high) when guess > actual, do: guess(actual, low..guess-1)
end

Chop.guess(273, 1..1000)

(1..10) |> Enum.map(&(&1*&1)) |> Enum.filter(&(&1 < 40))


:io.format("The number is ~3.1f~n", [5.678])
:io.format("The number is ~3.2f~n", [5.678])
:io.format("The number is ~2.2f~n", [5.678])
:io.format("The number is ~1.2f~n", [5.678])
:io.format("The number is ~0.2f~n", [5.678])
:io.format("The number is ~4.2f~n", [5.678])
:io.format("The number is ~5.2f~n", [5.678])

System.get_env("Path")
:io.format("~.2f~n", [2.0/3.0])
Path.extname("dave/test.exs")

System.cwd


[1,2,3] ++ [4,5,6]
List.flatten([[[1], 2], [[[3]]]])
List.foldl([1,2,3], "", fn value, acc -> "#{value}(#{acc})" end)
List.foldr([1,2,3], "", fn value, acc -> "#{value}(#{acc})" end)

List.replace_at(list, 2, "buckle my shoe")
List.keyfind(kw, "Dallas", 1)

Stream.cycle(~w{ green white }) |> Stream.zip(1..10) |> Enum.map(fn {class, value} -> "<tr class='#{class}'><td>#{value}</td></tr>\n" end) |> IO.puts

Stream.repeatedly(fn -> true end) |> Enum.take(3)

Stream.iterate(0, &(&1+1)) |> Enum.take(5)
Stream.iterate(2, &(&1*&1)) |> Enum.take(5)
Stream.iterate([], &[&1]) |> Enum.take(50)
Stream.unfold({0,1}, fn {f1,f2} -> {f1, {f2, f1+f2}} end) |> Enum.take(15)

defmodule Countdown do
  def sleep(seconds) do
   receive do
   after seconds*1000 -> nil
   end
  end
  
  def say(text) do
  spawn fn -> :os.cmd('say #{text}') end
  end
  def timer do
  Stream.resource(
     fn -> # the number of seconds to the start of the next minute
	 {_h,_m,s} = :erlang.time
       60 - s - 1
     end,

	 fn # wait for the next second, then return its countdown
      0 ->
        {:halt, 0}

	  count ->
        sleep(1)
        { [inspect(count)], count - 1 }
     end,
     
	 fn _ -> nil end # nothing to deallocate
    )
  end
end

counter = Countdown.timer
printer = counter |> Stream.each(&IO.puts/1)
speaker = printer |> Stream.each(&Countdown.say/1)


tax_rates = [ NC: 0.075, TX: 0.08 ]
orders = [
[ id: 123, ship_to: :NC, net_amount: 100.00 ],
[ id: 124, ship_to: :OK, net_amount: 35.50 ],
[ id: 125, ship_to: :TX, net_amount: 24.00 ],
[ id: 126, ship_to: :TX, net_amount: 44.80 ],
[ id: 127, ship_to: :NC, net_amount: 25.00 ],
[ id: 128, ship_to: :MA, net_amount: 10.00 ],
[ id: 129, ship_to: :CA, net_amount: 102.00 ],
[ id: 130, ship_to: :NC, net_amount: 50.00 ] ]



```
