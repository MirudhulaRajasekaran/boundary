# boundary
clc;
clear;
close all;

% Step 1: Read the input image
img = imread('skewed_document.jpg');
subplot(1, 3, 1), imshow(img), title('Original Image');

% Step 2: Convert to grayscale
gray = rgb2gray(img);

% Step 3: Edge detection using Canny
edges = edge(gray, 'Canny');
subplot(1, 3, 2), imshow(edges), title('Edge Detection');

% Step 4: Morphological operations to close gaps
se = strel('rectangle', [5, 5]);
closed = imclose(edges, se);
filled = imfill(closed, 'holes');
% (No subplot for closed/filled)

% Step 5: Find contours
[B, L] = bwboundaries(filled, 'noholes');
stats = regionprops(L, 'Area', 'BoundingBox', 'ConvexHull');

% Step 6: Select the largest boundary (assumed to be document)
[maxArea, idx] = max([stats.Area]);
documentBoundary = stats(idx).ConvexHull;

% Step 7: Show image with detected document boundary
subplot(1, 3, 3), imshow(img), title('Detected Document Corners');
hold on;
plot(documentBoundary(:,1), documentBoundary(:,2), 'g', 'LineWidth', 2);
hold off;
