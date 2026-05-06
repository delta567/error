EXP 1 : TECHNIQUES FOR CDMA


CODE:
import numpy as np
c1=[1,1,1,1]
c2=[1,-1,1,-1]
c3=[1,1,-1,-1]
c4=[1,-1,-1,1]
rc=[]
print("Enter the data bits :")
d1=int(input("Enter D1:"))
d2=int(input("Enter D2:"))
d3=int(input("Enter D3:"))
d4=int(input("Enter D4:"))
r1=np.multiply(c1,d1)
r2=np.multiply(c2,d2)
r3=np.multiply(c3,d3)
r4=np.multiply(c4,d4)
resultant_channel=r1+r2+r3+r4;
print("Resultant Channel",resultant_channel)
Channel=int(input("Enter the station to listen for C1=1,C2=2,C3=3,C4=4:"))

if Channel==1:rc=c1
elif Channel==2:rc=c2
elif Channel==3:rc=c3
elif Channel==4:rc=c4

inner_product=np.multiply(resultant_channel,rc)

print("Inner Product",inner_product)
res1=sum(inner_product)

data=res1/len(inner_product)

print("Data bit that was sent",data)








EXP 4 :

PART 1 - BPSK over AWGN (MATLAB Code)

CODE:

clear;
clc;

N = 10^6;
rng(100); % Set seed

% Generate random bits
ip = rand(1, N) > 0.5;

% BPSK modulation
s = 2*ip - 1;

% AWGN noise
n = 1/sqrt(2) * (randn(1, N) + 1i*randn(1, N));

Eb_N0_dB = -3:12;

for ii = 1:length(Eb_N0_dB)
    % Received signal
    y = s + 10^(-Eb_N0_dB(ii)/20) * n;

    % Detection
    ipHat = real(y) > 0;

    % Count errors
    nErr(ii) = sum(ip ~= ipHat);
end

% BER
simBer = nErr / N;

% Theoretical BER
theoryBer = 0.5 * erfc(sqrt(10.^(Eb_N0_dB/10)));

% Plot
figure;
semilogy(Eb_N0_dB, theoryBer, 'b-', 'LineWidth', 2);
hold on;
semilogy(Eb_N0_dB, simBer, 'mx-', 'LineWidth', 2);

axis([-3 12 1e-5 0.5]);
grid on;

legend('Theory', 'Simulation');
xlabel('Eb/No (dB)');
ylabel('Bit Error Rate');
title('BER for BPSK over AWGN');






PART 2 : BPSK over Rayleigh Channel
CODE :

clear;
clc;

N = 10^6;

% Transmitter
ip = rand(1, N) > 0.5; % random bits
s = 2*ip - 1;          % BPSK

Eb_N0_dB = -3:35;

for ii = 1:length(Eb_N0_dB)

    % Rayleigh channel
    h = 1/sqrt(2) * (randn(1, N) + 1i*randn(1, N));

    % AWGN noise
    n = 1/sqrt(2) * (randn(1, N) + 1i*randn(1, N));

    % Received signal
    y = h .* s + 10^(-Eb_N0_dB(ii)/20) * n;

    % Equalization
    yHat = y ./ h;

    % Detection
    ipHat = real(yHat) > 0;

    % Errors
    nErr(ii) = sum(ip ~= ipHat);
end

% Simulated BER
simBer = nErr / N;

% Theoretical BER (AWGN)
theoryBerAWGN = 0.5 * erfc(sqrt(10.^(Eb_N0_dB/10)));

% Theoretical BER (Rayleigh)
EbN0Lin = 10.^(Eb_N0_dB/10);
theoryBerRayleigh = 0.5 * (1 - sqrt(EbN0Lin ./ (EbN0Lin + 1)));

% Plot
figure;
semilogy(Eb_N0_dB, theoryBerAWGN, 'c-', 'LineWidth', 2);
hold on;
semilogy(Eb_N0_dB, theoryBerRayleigh, 'b-', 'LineWidth', 2);
semilogy(Eb_N0_dB, simBer, 'mx-', 'LineWidth', 2);

axis([-3 35 1e-5 0.5]);
grid on;

legend('AWGN Theory', 'Rayleigh Theory', 'Rayleigh Simulation');
xlabel('Eb/No (dB)');
ylabel('Bit Error Rate');
title('BER for BPSK in Rayleigh Channel');





// TCP IP

// client_file.py

import socket

HOST = '127.0.0.1'   # Server IP (same PC = localhost)
PORT = 5001

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect((HOST, PORT))

file = open("send_file.txt", "rb")

while True:
    data = file.read(1024)
    if not data:
        break
    client.sendall(data)

file.close()
client.close()

print("File sent successfully!")


// server_file.py

import socket

HOST = '0.0.0.0'   # Listen on all interfaces
PORT = 5001

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind((HOST, PORT))
server.listen(1)

print("Server is waiting for connection...")

conn, addr = server.accept()
print(f"Connected by {addr}")

file = open("received_file.txt", "wb")

while True:
    data = conn.recv(1024)
    if not data:
        break
    file.write(data)

file.close()
conn.close()
server.close()

print("File received successfully!")
