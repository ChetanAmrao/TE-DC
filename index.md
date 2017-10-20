
<!DOCTYPE html
  PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html><head>
      <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
   <!--
This HTML was auto-generated from MATLAB code.
To make changes, update the MATLAB code and republish this document.
      --><title>pam1</title><meta name="generator" content="MATLAB 8.5"><link rel="schema.DC" href="http://purl.org/dc/elements/1.1/"><meta name="DC.date" content="2017-10-21"><meta name="DC.source" content="pam1.m"><style type="text/css">
html,body,div,span,applet,object,iframe,h1,h2,h3,h4,h5,h6,p,blockquote,pre,a,abbr,acronym,address,big,cite,code,del,dfn,em,font,img,ins,kbd,q,s,samp,small,strike,strong,sub,sup,tt,var,b,u,i,center,dl,dt,dd,ol,ul,li,fieldset,form,label,legend,table,caption,tbody,tfoot,thead,tr,th,td{margin:0;padding:0;border:0;outline:0;font-size:100%;vertical-align:baseline;background:transparent}body{line-height:1}ol,ul{list-style:none}blockquote,q{quotes:none}blockquote:before,blockquote:after,q:before,q:after{content:'';content:none}:focus{outine:0}ins{text-decoration:none}del{text-decoration:line-through}table{border-collapse:collapse;border-spacing:0}

html { min-height:100%; margin-bottom:1px; }
html body { height:100%; margin:0px; font-family:Arial, Helvetica, sans-serif; font-size:10px; color:#000; line-height:140%; background:#fff none; overflow-y:scroll; }
html body td { vertical-align:top; text-align:left; }

h1 { padding:0px; margin:0px 0px 25px; font-family:Arial, Helvetica, sans-serif; font-size:1.5em; color:#d55000; line-height:100%; font-weight:normal; }
h2 { padding:0px; margin:0px 0px 8px; font-family:Arial, Helvetica, sans-serif; font-size:1.2em; color:#000; font-weight:bold; line-height:140%; border-bottom:1px solid #d6d4d4; display:block; }
h3 { padding:0px; margin:0px 0px 5px; font-family:Arial, Helvetica, sans-serif; font-size:1.1em; color:#000; font-weight:bold; line-height:140%; }

a { color:#005fce; text-decoration:none; }
a:hover { color:#005fce; text-decoration:underline; }
a:visited { color:#004aa0; text-decoration:none; }

p { padding:0px; margin:0px 0px 20px; }
img { padding:0px; margin:0px 0px 20px; border:none; }
p img, pre img, tt img, li img, h1 img, h2 img { margin-bottom:0px; } 

ul { padding:0px; margin:0px 0px 20px 23px; list-style:square; }
ul li { padding:0px; margin:0px 0px 7px 0px; }
ul li ul { padding:5px 0px 0px; margin:0px 0px 7px 23px; }
ul li ol li { list-style:decimal; }
ol { padding:0px; margin:0px 0px 20px 0px; list-style:decimal; }
ol li { padding:0px; margin:0px 0px 7px 23px; list-style-type:decimal; }
ol li ol { padding:5px 0px 0px; margin:0px 0px 7px 0px; }
ol li ol li { list-style-type:lower-alpha; }
ol li ul { padding-top:7px; }
ol li ul li { list-style:square; }

.content { font-size:1.2em; line-height:140%; padding: 20px; }

pre, code { font-size:12px; }
tt { font-size: 1.2em; }
pre { margin:0px 0px 20px; }
pre.codeinput { padding:10px; border:1px solid #d3d3d3; background:#f7f7f7; }
pre.codeoutput { padding:10px 11px; margin:0px 0px 20px; color:#4c4c4c; }
pre.error { color:red; }

@media print { pre.codeinput, pre.codeoutput { word-wrap:break-word; width:100%; } }

span.keyword { color:#0000FF }
span.comment { color:#228B22 }
span.string { color:#A020F0 }
span.untermstring { color:#B20000 }
span.syscmd { color:#B28C00 }

.footer { width:auto; padding:10px 0px; margin:25px 0px 0px; border-top:1px dotted #878787; font-size:0.8em; line-height:140%; font-style:italic; color:#878787; text-align:left; float:none; }
.footer p { margin:0px; }
.footer a { color:#878787; }
.footer a:hover { color:#878787; text-decoration:underline; }
.footer a:visited { color:#878787; }

table th { padding:7px 5px; text-align:left; vertical-align:middle; border: 1px solid #d6d4d4; font-weight:bold; }
table td { padding:7px 5px; text-align:left; vertical-align:top; border:1px solid #d6d4d4; }





  </style></head><body><div class="content"><h2>Contents</h2><div><ul><li><a href="#1">Creating Required Frequencies</a></li><li><a href="#2">Generating Square Wave With A Given Duty Cycle</a></li><li><a href="#3">Creating Our Message</a></li><li><a href="#4">PAM generation</a></li><li><a href="#5">Plotting it all one by one</a></li></ul></div><h2>Creating Required Frequencies<a name="1"></a></h2><pre class="codeinput"><span class="comment">%carrier frequency</span>
fc = 20;
<span class="comment">% message frequency</span>
fm = 2;
<span class="comment">% sampling frequency</span>
fs = 1000;
<span class="comment">%total time duration of simulation</span>
t = 1;

<span class="comment">%removing one sample from the end to fit the signal</span>
n = [0:1/fs:t];
n = n(1:end - 1);
</pre><h2>Generating Square Wave With A Given Duty Cycle<a name="2"></a></h2><p>setting a duty cycle</p><pre class="codeinput">duty = 20;
<span class="comment">% generating square wave with our duty cycle</span>
s = square(2*pi*fc*n,duty);
<span class="comment">% only keeping the positive part as we generate the pulse wave</span>
<span class="comment">% only from 0 to 1 V instead of -1 to 1 V</span>
s(s&lt;0) = 0;
</pre><h2>Creating Our Message<a name="3"></a></h2><pre class="codeinput">m = sin(2*pi*fm*n);<span class="comment">%message signal</span>

period_samp = length(n)/fc; <span class="comment">%created a sampling duration variable</span>

ind = [1:period_samp:length(n)];<span class="comment">%to know where are pulse starts</span>

on_samp = ceil(period_samp * duty/100);<span class="comment">%found the ON time of sample</span>
</pre><h2>PAM generation<a name="4"></a></h2><pre class="codeinput">pam = zeros(1,length(n));<span class="comment">%create PAM as a zeros vector with length of n</span>
<span class="keyword">for</span> i = 1 : length(ind)<span class="comment">%from zero to total index array length</span>
    pam(ind(i):ind(i) + on_samp) = m(ind(i));<span class="comment">%added our message with ON time</span>
    <span class="comment">%to PAM</span>
<span class="keyword">end</span>
</pre><h2>Plotting it all one by one<a name="5"></a></h2><p>plotting the pulsed wave</p><pre class="codeinput">subplot(2,2,1);plot(n,s);ylim([-0.2 1.2]);
ti1 = title(<span class="string">'Message signal'</span>);
ti1.Color = <span class="string">'blue'</span>;
xlabel ( <span class="string">' time(sec) '</span>);
ylabel (<span class="string">' Amplitude(volt)   '</span>);

<span class="comment">% plotting the message input signal</span>
subplot(2,2,2);plot(n,m,<span class="string">'r'</span>);ylim([-1.2 1.2]);
ti1 = title(<span class="string">'Message signal'</span>);
ti1.Color = <span class="string">'red'</span>;
xlabel ( <span class="string">' time(sec) '</span>);
ylabel (<span class="string">' Amplitude(volt)   '</span>);

<span class="comment">% plotting the PAM signal</span>
subplot(2,2,[3,4]);plot(n,pam,<span class="string">'g'</span>);ylim([-1.2 1.2]);
ti1 = title(<span class="string">'PAM signal'</span>);
ti1.Color = <span class="string">'green'</span>;
xlabel ( <span class="string">' time(sec) '</span>);
ylabel (<span class="string">' Amplitude(volt)   '</span>);
</pre><img vspace="5" hspace="5" src="pam1_01.png" alt=""> <p class="footer"><br><a>BYE!</a><br></p></div><!--
##### SOURCE BEGIN #####
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

##### SOURCE END #####
--></body></html>
