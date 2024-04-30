# SpeedCompare

💻 Basic network speed testing client & server scripts in Python

## Installation

```sh
pip install git+https://github.com/jcbhmr/speedcompare.git
```

## Usage

<table><td>

```sh
speedcompare-server tcp
```

<td>

```sh
speedcompare tcp
```

</table>

There are three modes for the client and the server: tcp, udp, and http11. The options are _mostly_ the same for each mode.

For the server there's `--bind` and `--size`. The server determines how much data to send to the client. The client just counts the data and ends the timer when the connection closes. For UDP since there isn't a connection close event that you can detect from the receiver we use a timeout instead.

On the client there's `--host` and `--sockets` to set the remote server address and the number of simultaneous connections to make. Each connection will be run in a new Python thread.

## How it works

You choose the amount of data to send from the server to the client. When a new client connects to the server it immediately sends that many bytes. The client then can check how long it took to receive the data and calculate the speed.

## Results

### UDP

All of these tests were run from localhost to localhost. There was no external network involved.

| Sockets | Loop delay | Data | Time | Speed | Percent |
| --- | --- | --- | --- | --- | --- |
| 1 | 0 ms | 10 MB | 1.02 s | 4.46 Mbps | 5.73% |
| 1 | 1 ms | 10 MB | 3.02 s | 25.51 Mbps | 96.54% |

Things I noticed:

- It's almost like the UDP server is sending the data _too fast_ and it just gets skipped over. For instance when you introduce a 1ms delay in the `send(4096 bytes)` loop the percent of data received goes up quite drastically!

## Development

<table><thead><tr><th>Linux & macOS<th>Windows
<tbody><tr><td>

```sh
ptyhon3.12 -m venv .venv
. .venv/bin/activate
```

<td>

```ps1
py -3.12 -m venv .venv
.venv\Scripts\Activate.ps1
```

</table>

```sh
pip install -r requirements.txt
```
