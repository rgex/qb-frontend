'use strict';

var myAngularApp = angular.module('qb', ['ngProgress']);

var sumChart = function(input, $sce, pxSize) {
    	input = input || [];
	var out = '<div class="chart">';
	var maximum = 0;
	for(var year in input)
	{
		if(maximum < input[year])
			maximum = input[year];
	}

	if(maximum === 0)
		return '';

	for(var year in input)
	{
		var px = pxSize - Math.round((input[year]/maximum) * pxSize);
		out = out + '<div class="column"><div class="column-buffer" style="height:' + px + 'px;"></div><div class="column-value">' + input[year] + '</div></div>';
	}

	out = out + '</div><div class="chart-years">';

	for(var year in input)
	{
		out = out + '<div class="chart-year">' + year + '</div>';
	}

	out = out + '</div>';

	return $sce.trustAsHtml(out);
  };

myAngularApp.filter('sumChart', ['$sce', function($sce) {
	return function(input) { return sumChart(input, $sce, 70); };
}]);

myAngularApp.filter('avgSumChart', ['$sce', function($sce) {
	return function(input) { return sumChart(input, $sce, 120); };
}]);


var indexController = myAngularApp.controller('IndexController', ['$scope', '$http', 'ngProgressFactory', function crtl($scope, $http, ngProgressFactory)
{
	$scope.progressbar = ngProgressFactory.createInstance();
	//$scope.progressbar.color('#0480be');

	$scope.resultsCount = 0;
	$scope.notSearchedYet = true;

	$scope.searchFilter = {};
	
	$scope.krankenhauser = [];

	$scope.totalCount = {};

	$scope.doFilter = function()
	{
		if(typeof $scope.searchFilter === "undefined"
		|| typeof $scope.searchFilter.opsIcd === "undefined"
		|| $scope.searchFilter.opsIcd.length === 0) {
			$scope.notSearchedYet = true;
			return;
		}
		$scope.progressbar.start();
		$http.post('http://163.172.129.98:3000/search/total', {"json":JSON.stringify($scope.searchFilter)}).success(function(response){
			$scope.krankenhauser = response.krankenhauserTotal;
			$scope.totalCount = response.total;
			$scope.resultsCount = response.krankenhauserTotal.length;
			$scope.progressbar.complete();
			$scope.notSearchedYet = false;
		});
	}
	
	/*
	$('#stadtInput').typeahead(null,
	{
	  name: 'cities',
	  remote: 'typeahead'
	});*/
	
	
}]);
