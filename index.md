%% Creating Required Frequencies
%carrier frequency
fc = 20;
% message frequency
fm = 2;
% sampling frequency
fs = 1000;
%total time duration of simulation
t = 1;

%removing one sample from the end to fit the signal
n = [0:1/fs:t];
n = n(1:end - 1);

%% Generating Square Wave With A Given Duty Cycle
% setting a duty cycle
duty = 20;
% generating square wave with our duty cycle
s = square(2*pi*fc*n,duty);
% only keeping the positive part as we generate the pulse wave
% only from 0 to 1 V instead of -1 to 1 V
s(s<0) = 0;
%% Creating Our Message
m = sin(2*pi*fm*n);%message signal

period_samp = length(n)/fc; %created a sampling duration variable

ind = [1:period_samp:length(n)];%to know where are pulse starts

on_samp = ceil(period_samp * duty/100);%found the ON time of sample

%% PAM generation
pam = zeros(1,length(n));%create PAM as a zeros vector with length of n
for i = 1 : length(ind)%from zero to total index array length
    pam(ind(i):ind(i) + on_samp) = m(ind(i));%added our message with ON time
    %to PAM
end

%% Plotting it all one by one
% plotting the pulsed wave
subplot(2,2,1);plot(n,s);ylim([-0.2 1.2]);
ti1 = title('Message signal');
ti1.Color = 'blue';
xlabel ( ' time(sec) ');
ylabel (' Amplitude(volt)   ');

% plotting the message input signal
subplot(2,2,2);plot(n,m,'r');ylim([-1.2 1.2]);
ti1 = title('Message signal');
ti1.Color = 'red';
xlabel ( ' time(sec) ');
ylabel (' Amplitude(volt)   ');

% plotting the PAM signal
subplot(2,2,[3,4]);plot(n,pam,'g');ylim([-1.2 1.2]);
ti1 = title('PAM signal');
ti1.Color = 'green';
xlabel ( ' time(sec) ');
ylabel (' Amplitude(volt)   ');
