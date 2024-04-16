# data-processing
Reduce the number of data points in a curve
>> % Load data from 'data.mat'
loadedData = load('data.mat');  % This loads all variables stored in the MAT file
data = struct2cell(loadedData); % Convert structure to cell array if needed
data = data{1}; % Assuming the data matrix is the first variable in the loaded data

% Curve names
curve_names = {'CL-UA-R', 'CL-UA-S', '4C18-UA-R', '4C18-UA-S', '4C30-UA-R', '4C30-UA-S'};

% Initialize a table to store all data
allData = table();

% Initialize figure for plotting
figure;
hold on;

% Process each pair of columns as one curve
for i = 1:6
    col_x = 2 * i - 1;
    col_y = 2 * i;
    
    % Extract x and y data for the current curve
    x = data(:, col_x);
    y = data(:, col_y);

    % Determine indices for approximately 30 evenly spaced data points
    indices = round(linspace(1, length(x), 30));
    reduced_x = x(indices);
    reduced_y = y(indices);
    
    % Plot the reduced data for visualization
    plot(reduced_x, reduced_y, '-o', 'DisplayName', curve_names{i});

    % Prepare the data for export
    curveData = table(reduced_x, reduced_y, 'VariableNames', {['X_' curve_names{i}], ['Y_' curve_names{i}]});
    
    % Append this curve's data to the allData table
    allData = [allData, curveData];
end

% Enhance the plot
legend('show');
xlabel('X-axis');
ylabel('Y-axis');
title('Extracted Data Points from Each Curve');
hold off;

% Export all curves' data into a single Excel file
writetable(allData, 'All_Curves_Data.xlsx');
