# edge-highlighter-1

%% concentration 0.5

%%
%'44.jpg'(128*128) is T1; '64.jpg'(256*256) is T2

A=imread('44.jpg');

% resize a 256*256 image to 128*128
I=imread('64.jpg');
B=imresize(I,0.5);

% change image from unit8 to double
A=double(A);
B=double(B);

figure, imshowpair(A,B,'montage');
title('initial images');

%% regular edge detector is not sensitive enough to hightlight the edge of contrast

%find edge using Canny/Prewitt method
 BW1=edge(A,'Canny');
 BW2=edge(B,'Prewitt');

 figure, imshowpair(BW1,BW2,'montage');
 title('Canny/Prewitt edge detector results')

%% find edge using our algorithm

%% average neighborhood

Aave= conv2(A, ones(3)/9, 'same');
Bave= conv2(B, ones(3)/9, 'same');
M=0;
N=0;
[a,b]=size(A);

%% pick the points & do algrithom
for n=2:1:a-1
    for k=2:1:b-1
        if A(n,k)>Aave(n,k)+2
            M=1;
        else
            M=0;
        end
        if B(n,k)<Bave(n,k)
            N=1;
        else 
            N=0;
        end
        if M*N==1
            A(n,k)=0;
        end
        end
end

%% show figure

% figure, imshow(A);

%% find edge of the fused image:

BW3=edge(A,'Canny');
BW4=edge(A,'Prewitt');

% figure, imshow(BW3)
% title ('edge highlighter algorithm')

%% compare the acquired edge of contrast with the real edge ('4.jpg'):

C=imread('4.jpg');
BW5=edge(C,'Canny');
BW6=edge(C,'Prewitt');

figure, imshowpair(BW3,BW6,'montage');
title('edge highlighter algorithm & clear contrast edge image')

figure, imshowpair(BW3,BW6);
title('comparison w/ clear contrast edge image')
