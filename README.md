# bfs1
%% To obtain BFS by Algebraic methods 
% Max Z=2x1+3x2+4x3+7x4 
% s.t. 2x1+3x2-x3+4x4=8

% s.t. x1-2x2+6x3-7x4=-3
clc 
clear all
format short 
%% Phase-1 Input the parameter
c=[2 3 4 7]; % objective function 
A=[2 3 -1 4; 1 -2 6 -7]; % coeff mat 
B=[8 ; -3]; % RHS of const
Objective=1 % 1 for max -1 for min problem
%% Phase-2 No of const and variable 
m=size(A,1); % no of const
n=size(A,2); % no of variable 
%% Phase-3 Compute the ncm Basic Solution
nab=nchoosek(n,m) % total number of atmost basic solution 
t=nchoosek(1:n,m) % pair of basic sol
%% Phase-4 Construct the basic solution 
sol=[]; % default solution is zero
if n>m
    for i=1:nab
    y=zeros(n,1);
    X=(A(:,t(i,:)))\B;
    %% To check the feasibilty condition 
    if all(X>=0 & X~=inf & X~=-inf)
        y(t(i,:))=X;
        sol=[sol y]
    end
    end
else
    error('number of variables is less than number of constraints')
end
sol
if any(X==0)
    fprintf('Degenerate solution')
else
    fprintf('Non-degenerate solution')
end
%% to find optimal solution 
Z=c*sol;
if Objective==1 
    [Zmax,Zindex]=max(Z);
    bfs=sol(:,Zindex);
    [Optimal_value]=[bfs' Zmax];
else
    [Zmin,Zindex]=min(Z);
    bfs=sol(:,Zindex);
    [Optimal_value]=[bfs' Zmin];
end
optimal_bfs=array2table(Optimal_value);
optimal_bfs.Properties.VariableNames(1:size(optimal_bfs,2))={'x1','x2','x3','x4','x5'}
