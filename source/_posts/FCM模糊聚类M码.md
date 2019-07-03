---
title: FCM模糊聚类M码
date: 2018-03-17 11:26:17
tags:
  - Matlab
categories:
  - 数学建模
---

源代码，本来用于数模题目中的电网设计

```matlab
function [clusterResult,U,iter_n] = FCM(upPresureSite, windMachineSites, clusterNum, m)
    % 输入：
    % clusterNum：聚类个数，1 < K < N（点个数）
    % m：模糊指数，1 < m < +无穷
    % upPresureSite：升压站坐标，1 x 2 矩阵
    % windMachineSites：风机坐标，N x 2 矩阵
    % maxWMNum：一串最大风机数，缺省时为5
    % minWMNum：一串最小风机数，缺省时为3
    
    %% 1- 初始化 U0 以及 初始化变量
    %     maxWMNum = 5;
    %     minWMNum = 3;
    [dataNum,dimention] = size(windMachineSites) ;
    upPresureSiteMatrix = repmat(upPresureSite,[dataNum,1]);
    windMachineSites = windMachineSites - upPresureSiteMatrix;  % 以升电站为原点
    epsilon = 1e-40; % 收敛精度，epsilon > 0
    iters = 10000 ;  % 模糊聚类迭代次数
    Uexpo = 2 ;  % 隶属度指数
    % 随机初始化 模糊划分矩阵：U0，使U0满足列上相加为1
    U0 = rand(clusterNum,dataNum);
    U0=U0./(ones(clusterNum,1)*sum(U0));
    U = U0 ;
    % 初始化聚类中心
    centerMatrix = zeros(clusterNum,dimention);
    % 初始化价值函数矩阵
    valueFunctionMatrix = zeros(iters,1);
    
    % FCM 循环开始
    for iterCount = 1:iters
        %% 2- 计算clusterNum 个聚类中心
        Uout = U .* Uexpo;  % 隶属度指数相乘
        centerMatrix = (Uout * windMachineSites) ./ sum(U(clusterNum,:));
        %% 3- 计算 价值函数
        % 3.1- 计算 距离矩阵
        distMatrix = zeros(clusterNum, dataNum);
        % 遍历clusterNum个聚类中心
        for clusterCount = 1:clusterNum
            % 求当前聚类中心求与所有样本点距离
            for dataCount = 1:dataNum
                distMatrix(clusterCount,dataCount) = ...
                norm( centerMatrix(clusterCount,:) - windMachineSites(dataCount,:) );
            end
        end
        % 3.2- 计算价值函数 值
        distMatrix2 = distMatrix .^ 2;  % 平方
        valueFunctionMatrix(iterCount) = sum(sum(distMatrix2 .* Uout));
        %% 4- 计算新的 模糊划分矩阵U
        for i = 1:clusterNum
            for j = 1:dataNum
                DijVector = distMatrix(i,j) * ones(clusterNum,1);
                DNjVector = distMatrix(:,j);
                temp = (DijVector ./ DNjVector).^(2/(Uexpo-1));
                temp = sum(temp);
                valueFunctionMatrix(i,j) = 1 / temp;
            end
        end
        %% 5- 算法终止条件判断
        if iterCount > 1 && abs(valueFunctionMatrix(iterCount) - valueFunctionMatrix(iterCount-1)) <= epsilon
            break;
        end
    end
    iter_n = iterCount; % 实际迭代次数
    [max_U,clusterAns]=max(U);
    clusterResult = [ windMachineSites+upPresureSiteMatrix , clusterAns' ] ;
```