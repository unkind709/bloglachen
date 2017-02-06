# bloglachen
//===================== Start Material List =========================
        var reqmateriallist = {};
        // $scope.plantCode = null;
        $scope.locationCode = null;
        $scope.groupCode = null;
        $scope.materiallistdatas = null;

        $scope.checkcriteria = function(location, group, action) {
            // if (typeof plant != 'undefined' && plant != null) {
            //     $scope.plantCode = plant.plantCode;
            // }
            console.log(location);
            if (typeof location != 'undefined' && location != null && location != '') {
                $scope.locationCode = location.locationCode;
                if ($rootScope.locationCode != 'undefined' && $rootScope.locationCode != null && $rootScope.locationCode != '') {
                    //   if ($scope.locationCode != $rootScope.locationCode && $rootScope.totalmaterial > 0) {
                    if ($scope.locationCode != $rootScope.locationCode && $rootScope.materialrequests.length > 0) {

                        SweetAlert.swal({
                                title: "การเปลี่ยนคลังสินค้าต้องทำการยกเลิกอุปกรณ์ทั้งหมด?",
                                text: "",
                                type: "warning",
                                showLoaderOnConfirm: true,
                                showCancelButton: true,
                                confirmButtonColor: "#DD6B55",
                                confirmButtonText: "ตกลง",
                                cancelButtonText: "ยกเลิก",
                                closeOnConfirm: false,
                                closeOnCancel: true
                            },
                            function(isConfirm) {
                                if (isConfirm) {
                                    $rootScope.totalmaterial = 0;
                                    $rootScope.totalprice = 0;
                                    $rootScope.materialrequests = [];
                                    swal("เปลี่ยนคลังสินค้าเรียบร้อย", "", "success");
                                } else {
                                    $scope.ddllocationselected = { locationCode: $rootScope.locationCode };
                                    $scope.locationCode = $rootScope.locationCode;

                                    // swal("ยกเลิก", "", "error");
                                }
                            });
                    }
                }
            } else {
                $scope.locationCode = undefined;
                if (action) {
                    SweetAlert.swal({
                        title: "กรุณาเลือกคลังสินค้า",
                        text: "",
                        type: "error"
                    });
                }
            }
            if (typeof group != 'undefined' && group != null) {
                $scope.groupCode = group.groupCode;
            } else {
                $scope.groupCode = undefined;
            }

            if ($scope.locationCode != null && $rootScope.materialrequests.length <= 0) {
                if ($scope.txtsearchmaterial == '') {
                    $scope.txtsearchmaterial = undefined;
                }
                $scope.txtsearchmaterials = $scope.txtsearchmaterial;
                $scope.currentPage = 1;
                reqmateriallist = {
                    "requestData": {
                        "pageSize": $scope.pageSize,
                        "pageNumber": $scope.currentPage,
                        "sortBy": "mtDesc",
                        "sortType": "ASC",
                        "criteria": {
                            // "plantCode": $scope.plantCode,
                            "locationCode": $scope.locationCode,
                            "groupCode": $scope.groupCode,
                            "mtDesc": $scope.txtsearchmaterials
                        }
                    }
                }
                console.log(reqmateriallist);
                postMaterialList(reqmateriallist);

            }
        }

        $scope.searchmaterial = function() {
            console.log($scope.txtsearchmaterial);
            if ($scope.txtsearchmaterial == '') {
                $scope.txtsearchmaterial = undefined;
            }
            reqmateriallist = {
                "requestData": {
                    "pageSize": $scope.pageSize,
                    "pageNumber": $scope.currentPage,
                    "sortBy": "mtDesc",
                    "sortType": "ASC",
                    "criteria": {
                        // "plantCode": $scope.plantCode,
                        "locationCode": $scope.locationCode,
                        "groupCode": $scope.groupCode,
                        "mtDesc": $scope.txtsearchmaterials
                    }
                }
            }

            postMaterialList(reqmateriallist);
        }

        var postMaterialList = function(reqdata) {
            materialListService.getMaterialList(reqdata).then(function(response) {
                $scope.materiallistdatas = response.data.responseData;
                if ($scope.materiallistdatas != null) {
                    $scope.totalRecord = response.data.responseData.totalRecord;
                    $scope.totalPage = Math.ceil($scope.totalRecord / $scope.pageSize);
                }
                console.log(response.data.responseData);
            }, function(err) {
                console.log(err);
            });
        };
        //===================== End Material List =========================
