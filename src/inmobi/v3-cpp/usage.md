### 初始化 InMobi
* 在您的代码合适的地方初始化插件, 我们建议您在 `AppDelegate::applicationDidFinishLaunching()` 或 `AppController:didFinishLaunchingWithOptions()` 中完成初始化. 请确保您包含了对应的头文件:
```cpp
#include "PluginInMobi/PluginInMobi.h"
AppDelegate::applicationDidFinishLaunching()
{
     sdkbox::PluginInMobi::init();
}
```

### 显示插屏广告

```cpp
// 手动加载广告
sdkbox::PluginInMobi::loadInterstitial();

if (sdkbox::PluginInMobi::isInterstitialReady()) {
    CCLOG("Plugin InMobi interstitial ad is ready");
    sdkbox::PluginInMobi::showInterstitial();
} else {
    CCLOG("Plugin InMobi interstitial ad is not ready");
}
```

### 设置日志等级

```cpp
sdkbox::PluginInMobi::setLogLevel(sdkbox::PluginInMobi::SBIMSDKLogLevel::kIMSDKLogLevelDebug);
```

### 设置用户数据

```cpp
sdkbox::PluginInMobi::addIdForType("test", sdkbox::PluginInMobi::SBIMSDKIdType::kIMSDKIdTypeLogin);
sdkbox::PluginInMobi::removeIdType(sdkbox::PluginInMobi::SBIMSDKIdType::kIMSDKIdTypeLogin);
sdkbox::PluginInMobi::setAge(18);
sdkbox::PluginInMobi::setAreaCode("900");
sdkbox::PluginInMobi::setAgeGroup(sdkbox::PluginInMobi::SBIMSDKAgeGroup::kIMSDKAgeGroupBetween18And20);
sdkbox::PluginInMobi::setYearOfBirth(1989);
sdkbox::PluginInMobi::setEducation(sdkbox::PluginInMobi::SBIMSDKEducation::kIMSDKEducationHighSchoolOrLess);
sdkbox::PluginInMobi::setEthnicity(sdkbox::PluginInMobi::SBIMSDKEthnicity::kIMSDKEthnicityHispanic);
sdkbox::PluginInMobi::setGender(sdkbox::PluginInMobi::SBIMSDKGender::kIMSDKGenderMale);
sdkbox::PluginInMobi::setHouseholdIncome(sdkbox::PluginInMobi::SBIMSDKHouseholdIncome::kIMSDKHouseholdIncomeBelow5kUSD);
sdkbox::PluginInMobi::setIncome(4500);
sdkbox::PluginInMobi::setInterests("game");
sdkbox::PluginInMobi::setLanguage("en-us");
sdkbox::PluginInMobi::setLocation("cd", "sc", "usa");
sdkbox::PluginInMobi::setLocation(102, 348);
sdkbox::PluginInMobi::setNationality("nationality");
sdkbox::PluginInMobi::setPostalCode("618000");
```

### 接收 InMobi 事件 (可选)

* 让您的类继承 `sdkbox::InMobiListener`
```cpp
#include "PluginInMobi/PluginInMobi.h"
class MyClass : public sdkbox::InMobiListener {
public:
  	void bannerDidFinishLoading() {
        CCLOG("bannerDidFinishLoading");
    };
    void bannerDidFailToLoadWithError(sdkbox::PluginInMobi::SBIMStatusCode code, const std::string& description) {
        CCLOG("bannerDidFailToLoadWithError status:%d, desc:%s", code, description.c_str());
    };

    void bannerDidInteractWithParams(const std::map<std::string, std::string>& params) {
        CCLOG("bannerDidInteractWithParams");
    };

    void userWillLeaveApplicationFromBanner() {
        CCLOG("userWillLeaveApplicationFromBanner");
    };

    void bannerWillPresentScreen() {
        CCLOG("bannerWillPresentScreen");
    };

    void bannerDidPresentScreen() {
        CCLOG("bannerDidPresentScreen");
    };

    void bannerWillDismissScreen() {
        CCLOG("bannerWillDismissScreen");
    };

    void bannerDidDismissScreen() {
        CCLOG("bannerDidDismissScreen");
    };

    void bannerRewardActionCompletedWithRewards(const std::map<std::string, std::string>& rewards) {
        CCLOG("bannerRewardActionCompletedWithRewards");
    };

    void interstitialDidFinishLoading() {
        CCLOG("interstitialDidFinishLoading");
    };

    void interstitialDidFailToLoadWithError(sdkbox::PluginInMobi::SBIMStatusCode code, const std::string& description) {
        CCLOG("interstitialDidFailToLoadWithError status:%d, desc:%s", code, description.c_str());
    };

    void interstitialWillPresent() {
        CCLOG("interstitialWillPresent");
    };

    void interstitialDidPresent() {
        CCLOG("interstitialDidPresent");
    };

    void interstitialDidFailToPresentWithError(sdkbox::PluginInMobi::SBIMStatusCode code, const std::string& description) {
        CCLOG("interstitialDidFailToPresentWithError");
    };

    void interstitialWillDismiss() {
        CCLOG("interstitialWillDismiss");
    };

    void interstitialDidDismiss() {
        CCLOG("interstitialDidDismiss");
    };

    void interstitialDidInteractWithParams(const std::map<std::string, std::string>& params) {
        CCLOG("interstitialDidInteractWithParams");
    };

    void interstitialRewardActionCompletedWithRewards(const std::map<std::string, std::string>& rewards) {
        CCLOG("interstitialRewardActionCompletedWithRewards");
    };

    void userWillLeaveApplicationFromInterstitial() {
        CCLOG("userWillLeaveApplicationFromInterstitial");
    };
};
```

* 创建一个监听类来接收回调:
```cpp
sdkbox::PluginInMobi::setListener(this);
```
