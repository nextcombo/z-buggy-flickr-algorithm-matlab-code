# [z] Buggy Flickr Algorithm (Matlab Code)

1. The assumption is that all fireflies are hermaphroditic, so a single firefly may be attracted by any other firefly.
2. The brightness of the firefly determines its attractiveness; brighter fireflies attract darker ones. If there are no fireflies brighter than the one being considered, it will randomly move.
3. The optimal value of the function is directly proportional to the brightness of the firefly.
📚2 Run Result Editing
Edit
Edit
Edit
Edit
Part of the code:
clc;
clear;
warning('off');
% Data Loading
data=JustLoad();
% Generate Fuzzy Model
ClusNum=4; % Number of Clusters in FCM
% 
fis=GenerateFuzzy(data,ClusNum);
% 
%% Tarining FireFly Algorithm
FireFlyFis=FireFlyRegression(fis,data);
%% Plot Fuzzy FireFly Results (Train - Test)
% Train Output Extraction
TrTar=data.TrainTargets;
TrainOutputs=evalfis(data.TrainInputs,FireFlyFis);
% Test Output Extraction
TsTar=data.TestTargets;
TestOutputs=evalfis(data.TestInputs,FireFlyFis);
% Train calc  
Errors=data.TrainTargets-TrainOutputs;  
MSE=mean(Errors.^2);RMSE=sqrt(MSE);  
error_mean=mean(Errors);error_std=std(Errors);  
% Test calc  
Errors1=data.TestTargets-TestOutputs;  
MSE1=mean(Errors1.^2);RMSE1=sqrt(MSE1);  
error_mean1=mean(Errors1);error_std1=std(Errors1);  
% Train  
figure('units','normalized','outerposition',[0 0 1 1])  
subplot(3,2,1);  
plot(data.TrainTargets,'c');hold on;  
plot(TrainOutputs,'k');legend('Target','Output');  
title('FireFly Training Part');xlabel('Sample Index');grid on;  
% Test  
subplot(3,2,2);  
plot(data.TestTargets,'c');hold on;  
plot(TestOutputs,'k');legend('FireFly Target','FireFly Output');  
title('FireFly Testing Part');xlabel('Sample Index');grid on;  
% Train  
subplot(3,2,3);  
plot(Errors,'k');legend('FireFly Training Error');  
title(['Train MSE =  ' num2str(MSE) ' ,  Train RMSE =  ' num2str(RMSE)]);grid on;  
% Test  
subplot(3,2,4);  
plot(Errors1,'k');legend('FireFly Testing Error');  
title(['Test MSE =  ' num2str(MSE1) ' ,  Test RMSE =  ' num2str(RMSE1)]);grid on;  
% Train  
subplot(3,2,5);  
h=histfit(Errors, 50);h(1).FaceColor = [.8 .8 0.3];  
title(['Train Error Mean = ' num2str(error_mean) ' , Train Error STD = ' num2str(error_std)]);  
% Test  
subplot(3,2,6);  
h=histfit(Errors1, 50);h(1).FaceColor = [.8 .8 0.3];  
title(['Test Error Mean = ' num2str(error_mean1) ' , Test Error STD =  ' num2str(error_std1)]);  
%% Plot Just Fuzzy Results (Train - Test)  
% Train Output Extraction  
fTrainOutputs=evalfis(data.TrainInputs,fis);  
% Test Output Extraction  
fTestOutputs=evalfis(data.TestInputs,fis);  
% Train calc  
fErrors=data.TrainTargets-fTrainOutputs;  
fMSE=mean(fErrors.^2);fRMSE=sqrt(fMSE);  
ferror_mean=mean(fErrors);ferror_std=std(fErrors);  
% Test calc  
fErrors1=data.TestTargets-fTestOutputs;  
fMSE1=mean(fErrors1.^2);fRMSE1=sqrt(fMSE1);  
ferror_mean1=mean(fErrors1);ferror_std1=std(fErrors1);  
% Train  
figure('units','normalized','outerposition',[0 0 1 1])  
subplot(3,2,1);  
plot(data.TrainTargets,'m');hold on;  
plot(fTrainOutputs,'k');legend('Target','Output');  
title('Fuzzy Training Part');xlabel('Sample Index');grid on;  
% Test  
subplot(3,2,2);  
plot(data.TestTargets,'m');hold on;  
plot(fTestOutputs,'k');legend('Target','Output');  
title('Fuzzy Testing Part');xlabel('Sample Index');grid on;  
% Train  
subplot(3,2,3);  
plot(fErrors,'g');legend('Fuzzy Training Error');  
title(['Train MSE =  ' num2str(fMSE) ' ,  Test RMSE =  ' num2str(fRMSE)]);grid on;  
% Test  
subplot(3,2,4);  
plot(fErrors1,'g');legend('Fuzzy Testing Error');  
title(['Train MSE =  ' num2str(fMSE1) ' ,  Test RMSE =  ' num2str(fRMSE1)]);grid on;  
% Train  
subplot(3,2,5);  
h=histfit(fErrors, 50);h(1).FaceColor = [.3 .8 0.3];  
title(['Train Error Mean =  ' num2str(ferror_mean) ' , Train Error STD =  ' num2str(ferror_std)]);  
% Test  
subplot(3,2,6);  
h=histfit(fErrors1, 50);h(1).FaceColor = [.3 .8 0.3];  
title(['Test Error Mean =  ' num2str(ferror_mean1) ' , Test Error STD =  ' num2str(ferror_std1)]);  
%% Regression Plots  
figure('units','normalized','outerposition',[0 0 1 1])  
subplot(2,2,1)  
[population2,gof] = fit(TrTar,TrainOutputs,'poly4');  
plot(TrTar,TrainOutputs,'o',...  
'LineWidth',1,...  
'MarkerSize',6,...  
'MarkerEdgeColor','g',...  
'MarkerFaceColor',[0.9,0.1,0.1]);  
title(['FireFly Train - R = ' num2str(1-gof.rmse)]);  
xlabel('Train Target');  
ylabel('Train Output');  
hold on  
plot(population2,'b-','predobs');  
xlabel('Train Target');  
ylabel('Train Output');  
hold off  
subplot(2,2,2)  
[population2,gof] = fit(TsTar, TestOutputs,'poly4');  
plot(TsTar, TestOutputs,'o',...  
'LineWidth',1,...  
'MarkerSize',6,...  
'MarkerEdgeColor','g',...  
'MarkerFaceColor',[0.9,0.1,0.1]);  
title(['FireFly Test - R = ' num2str(1-gof.rmse)]);  
xlabel('Test Target');  
ylabel('Test Output');  
hold on  
plot(population2,'b-','predobs');  
xlabel('Test Target');  
ylabel('Test Output');  
hold off  
subplot(2,2,3)  
[population2,gof] = fit(TrTar,fTrainOutputs,'poly4');  
plot(TrTar,fTrainOutputs,'o',...  
% Fuzzy Train - R = 1 - Gof.rmse
title(['Fuzzy Train - R = ' num2str(1-gof.rmse)]);

% Plotting population vs. predicted output
xlabel('Train Target');
ylabel('Train Output');
hold on
plot(population2,'r-','predobs');
xlabel('Train Target');
ylabel('Train Output');
hold off

% Subplot for Gof analysis
subplot(2,2,4)
[population2, gof] = fit(TsTar, fTestOutputs,'

% References
[1] Matlab Central: [https://www.mathworks.com/matlabcentral/fileexchange/fuzzy-train-algorithm-tutorial](https://www.mathworks.com/matlabcentral/fileexchange/fuzzy-train-algorithm-tutorial)
[2] Fuzzy Train Algorithm Tutorial: [https://www.mathworks.com/help/optim/fuzzy_train_algorithm_tutorial.html](https://www.mathworks.com/help/optim/fuzzy_train_algorithm_tutorial.html)
[3] Fuzzy Train Algorithm: [https://www.mathworks.com/help/optim/fuzzy_train_algorithm.html](https://www.mathworks.com/help/optim/fuzzy_train_algorithm.html)

## Images

![[z] Buggy Return to Origin Algorithm (Matlab Code Implementation)](01.gif)
![[z] Buggy Return Algorithm of Fireflies (Matlab Code)](02.png)
![[z] Insects' fluorescence regression algorithm (Matlab code implementation)](03.png)
![[z] Buggy Return Algorithm of Firefly (Matlab Code)](04.png)
![[z] Buggy Return Algorithm of Fireflies (Matlab Code Implementation)](05.png)

<!-- PAYMENT_BLOCK -->

## Get this dataset

- Payment link (Stripe): [Buy now](https://buy.stripe.com/3cs8yP7sY87d0vu9AB)
- Payment QR code: ![Payment QR code](pay.png)

<!-- /PAYMENT_BLOCK -->
