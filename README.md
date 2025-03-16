- 👋 Hi, I’m @A2019356
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
A2019356/A2019356 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

The GunBound Archives
Log in
 Trusted Releases
GunBound TH Ranking Script
 Thread starterZilch  Start dateDec 5, 2024
Zilch
Zilch
Administrator Staff member
Dec 5, 2024
#1
This is the same script that's included in the GunBound Retro Server files, but for easy convenience. Script by me.

PHP:
<?php

/* -------------------------------------------------- */
/* GunBound Thor's Hammer Ranking Script by @zilchfox */
/* This Script Updates Total, Season, and Guild Ranks */
/* -------------------------------------------------- */

header('Content-Type: text/plain; charset=utf-8');
include "../functions/config.php";
//For MySQL Connections. If you do not have your own MySQL config.php, just simply use mysql_connect(); here.


$runguildupdate = 1; //Guild Updating disabled for now

echo "GunBound Rank Update System Created by @zilchfox\n";
echo "This script is intended to be run as a cron job in command prompt for maximum performance.\n\n";

//First things first, we need to remove inactive users from the ranking. Typically this is anyone who hasn't played in 90 days.
echo " >>> Starting Removing Inactive Users Script.......";
//It's... a very simple script, fortunately.
mysql_query("UPDATE `Game` SET `NoRankUpdate`=1 WHERE `LastUpdateTime` <= (NOW() - INTERVAL 90 DAY) AND `NoRankUpdate`=0 AND `LastUpdateTime`!=0") or die(mysql_error());
//That really should be it.
echo "OK\n\n";

//Then, we need to reinstate users who were previously inactive but are active again.
echo " >>> Starting Ranking Reinstate Script.......\n";
//We need to keep in mind anyone who is banned though, we should not be reinstating banned users or GMs.
//So... any user with Authority less than 0 or greater than 98 should never be included.

$sql_reinstate = mysql_query("SELECT `Game`.`NoRankUpdate`, `Game`.`LastUpdateTime`, `GunWcUser`.`Authority` AS `Authority`, `Game`.`Id` FROM `Game` INNER JOIN `GunWcUser` ON `Game`.`Id`=`GunWcUser`.`Id` WHERE `Game`.`NoRankUpdate`=1 AND `Game`.`LastUpdateTime` BETWEEN NOW() - INTERVAL 90 DAY AND NOW() AND `Authority` BETWEEN 0 AND 98");
while($sqlly_reinstate = mysql_fetch_row($sql_reinstate)){
    echo "Found User ". $sqlly_reinstate[3] . "...";
    mysql_query("UPDATE `Game` SET `NoRankUpdate`=0 WHERE `Id`='".$sqlly_reinstate[3]."'") or die(mysql_error());
    echo "Rank has been reinstated.\n";
}
echo "OK\n\n";

//We need to check and see if we've done our daily rank update (24 hrs)
$sql_daily = mysql_query("SELECT COUNT(*) FROM `Game` WHERE `DailyRankUpdate` >= NOW() + INTERVAL -1 DAY
AND `DailyRankUpdate` <  NOW() + INTERVAL 0 DAY AND `NoRankUpdate`=0") or die(mysql_error());
$sqlly_daily = mysql_fetch_row($sql_daily);
echo " >>> Starting Daily Rank History Script.......\n";
if($sqlly_daily[0] > 0){
    //We have already updated this in the last day or so
    echo ">> Daily Rank History has been skipped this time.\n\n";
} else {
    
    mysql_query("UPDATE `Game` SET `TotalScoreHistory`=`TotalScore`, `SeasonScoreHistory`=`SeasonScore`, `TotalRankHistory`=`TotalRank`, `SeasonRankHistory`=`SeasonRank`, `DailyRankUpdate`=NOW()") or die(mysql_error());
    echo ">> Daily Rank History Updated Successfully.\n\n";
}

foreach (array("Total", "Season") as $ranktype){   
    echo " >>> Starting ".$ranktype." Rank Update Script.......\n";
    
    $count = 0; //Need to reset the count each time it's run
    /* Updating Rank Number */
    echo "[Updating ".$ranktype." rank system]\n";
    $rankSelect = mysql_query("SELECT * FROM `Game` WHERE `NoRankUpdate`=0 ORDER BY `".$ranktype."Score` DESC") or die(mysql_error());   
    while ($rank = mysql_fetch_array($rankSelect)){
        $count++;
        mysql_query("UPDATE `Game` SET `".$ranktype."Rank`='".$count."' WHERE `Id`='".$rank["Id"]."'") or die(mysql_error());
    }
    echo "> ".$ranktype."Rank Updated successfully for $count accounts.\n\n";

    /* Updating Lower Rank Grades */
    echo "[Updating lower ".$ranktype." grading system]\n";
    
    $lowergrade = array (
          array("A little Chick", 19, -2147483647, 1099),
        array("Wooden Hammer", 18, 1100, 1199),
        array("Double Wooden Hammer", 17, 1200, 1499),
        array("Stone Hammer", 16, 1500, 1799),
        array("Double Stone Hammer", 15, 1800, 2299),
        array("Metal Axe", 14, 2300, 2799),
        array("Double Metal Axe", 13, 2800, 3499),
        array("Silver Axe", 12, 3500, 4199),
        array("Double Silver Axe", 11, 4200, 5099),
        array("Gold Axe", 10, 5100, 5999),
        array("Double Gold Axe", 9, 6000, 6899));
    foreach ($lowergrade as $gradeupdate){
        echo "> Updating ". $gradeupdate[0] ."...";
        mysql_query("UPDATE `Game` SET `".$ranktype."Grade`=$gradeupdate[1] WHERE `".$ranktype."Score` BETWEEN $gradeupdate[2] AND $gradeupdate[3] AND `NoRankUpdate`=0") or die(mysql_error());
        echo "OK\n";
    }
    echo ">> Lower ".$ranktype."Grade Updated successfully.\n\n";
    
    /* Updating Upper Rank Grades */
    echo "[Updating upper ".$ranktype." grading system...]\n";
    
    $uppergrade = array (
          array("Battle Axe", 8, 1),
          array("Battle Axe+", 7, 0.7),
          array("Silver Battle Axe", 6, 0.5),
          array("Silver Battle Axe+", 5, 0.3),
          array("Gold Battle Axe", 4, 0.2),
          array("Gold Battle Axe+", 3, 0.1),
          array("Violet Wand", 2, 0.06),
          array("Sapphire Wand", 1, 0.03),
          array("Ruby Wand", 0, 0.01),
          array("Diamond Wand", -1, 0.002));
    
    $gcount = mysql_fetch_array(mysql_query("SELECT COUNT(*) FROM `Game` WHERE `".$ranktype."Rank`>21 AND `".$ranktype."Score`>6899 AND `NoRankUpdate`=0"));
    echo "> ".$gcount[0] ." accounts above 6899 GP and below rank 21 have been detected.\n";
    
    foreach ($uppergrade as $hgradeupdate){
        echo "> Updating ". $hgradeupdate[0] ."...";
                echo "Percentage is top ". ($hgradeupdate[2]*100) ."%...";

        mysql_query("UPDATE `Game` SET `".$ranktype."Grade`=".$hgradeupdate[1]." WHERE `".$ranktype."Score`>6899 AND `".$ranktype."Rank`>=22 AND `NoRankUpdate`=0 ORDER BY `".$ranktype."Score` DESC LIMIT ".(ceil($gcount[0] * $hgradeupdate[2]))."") or die(mysql_error());
        echo "OK\n";
    }
    echo ">> Upper ".$ranktype."Grade Updated successfully.\n\n";

    /* Updating Dragon Rank Grades */
    echo "[Updating Dragon ".$ranktype." grading system...]\n";
    
    $dragongrade = array (
        array("Blue Dragon", -2, 21, 6),
        array("Red Dragon", -3, 5, 2),
        array("Silver Dragon", -4, 1, 0));
    
    $gcount = mysql_fetch_array(mysql_query("SELECT COUNT(*) FROM `Game` WHERE `".$ranktype."Rank`<22 AND `".$ranktype."Score`>6899 AND `NoRankUpdate`=0"));
    echo "> ".$gcount[0] ." accounts above 6899 GP and above rank 21 have been detected.\n";
    if($gcount[0]<21){
    echo "> WARNING: There were less than 21 users detected with more than 6899 GP.\n";
    echo "> WARNING: Only the users with more than 6899 GP are eligible to be dragons.\n";
    }
    
    foreach ($dragongrade as $dgradeupdate){
        echo "> Updating ". $dgradeupdate[0]."...";       
        mysql_query("UPDATE `Game` SET `".$ranktype."Grade`='$dgradeupdate[1]' WHERE `".$ranktype."Rank` BETWEEN '$dgradeupdate[3]' AND '$dgradeupdate[2]' AND `NoRankUpdate`=0 AND `".$ranktype."Score`>6899") or die(mysql_error());
        echo "OK\n";       
    }
    echo ">> Dragon ".$ranktype."Grade Updated successfully.\n\n";

    /*Ensure there is always at least one rank of each kind above 6899 GP... as long as there's at least two of them.*/
    echo "[Auto-promoting top user of all percentage rank groups under Diamond Wand as long as there are 2+ users...]\n";
    $level_tree = array (
    array("Battle Axe", 6),
    array("Battle Axe+", 7),
    array("Silver Battle Axe", 6),
    array("Silver Battle Axe+", 5),
    array("Gold Battle Axe", 4),
    array("Gold Battle Axe+", 3),
    array("Violet Wand", 2),
    array("Sapphire Wand", 1),
    array("Ruby Wand", 0));

    foreach ($level_tree as $lvcheck){
    echo "Checking ".$lvcheck[0]."...";
    $check_count = mysql_query("SELECT COUNT(*) FROM `Game` WHERE `".$ranktype."Grade`='".$lvcheck[1]."'") or die(mysql_error());
    while($fincheck = mysql_fetch_row($check_count)){
    if ($fincheck[0] > 1){
        //Continue with script
        $all_that_rank = mysql_query("SELECT `Id`,`".$ranktype."Grade` FROM `Game` WHERE `".$ranktype."Grade`='".$lvcheck[1]."' ORDER BY `".$ranktype."Score` DESC LIMIT 1") or die(mysql_error());
        while($run_rank_change = mysql_fetch_array($all_that_rank)){
            echo "...FOUND $fincheck[0] USER TO UPDATE";
            mysql_query("UPDATE `Game` SET `".$ranktype."Grade`='".($lvcheck[1]-1)."' WHERE `Id`='".$run_rank_change["Id"]."'") or die(mysql_error());
            echo "UPDATE `Game` SET `".$ranktype."Grade`='".($lvcheck[1]-1)."' WHERE `Id`='".$run_rank_change["Id"]."'";
            //That should do it...hopefully.
        }
    } else {
        //Do nothing, because 1 user or less was detected
    }
    echo "done.\n";
}
}

echo ">> Promote top user complete.\n\n";


}

if($runguildupdate == 1){

    /* Update Guild Ranking */
    echo "[Updating guild ranking system...]\n";

    echo "> Update total members for all guilds (this will take a while)...";
    $sql = mysql_query("SELECT `GuildName` FROM `Guild` WHERE `Active`=1");
    while($sqlly = mysql_fetch_array($sql)){
        $gcount = mysql_query("SELECT `Id`,`TotalScore` FROM `Game` WHERE `Guild`='".$sqlly["GuildName"]."' ORDER BY `TotalRank` ASC") or die(mysql_error());
       $memberrank = 0;
        $guildscore = 0;
        while ($guildcount = mysql_fetch_array($gcount)){
            $memberrank++;
            $guildscore = $guildscore + $guildcount[1];
            if($guildcount[0] > 999){
                $guildcount[0] = 999; //The game only supports up to a maximum of 999 users per guild.
            }
            if($memberrank > 999){
                $memberrank = 999; //Same reason as above, force anyone above lower than rank 999 to stay at rank 999 together.
            }
            $totalgcount = mysql_query("SELECT COUNT(*) FROM `Game` WHERE `Guild`='".$sqlly["GuildName"]."'") or die(mysql_error());
            $totalg = mysql_fetch_row($totalgcount);   
            mysql_query("UPDATE `Game` SET `MemberCount`='".$totalg[0]."',`GuildRank`='".$memberrank."' WHERE `Id`='".$guildcount["Id"]."'") or die(mysql_error());
            mysql_query("UPDATE `Guild` SET `GuildScore`='".round($guildscore/100)."', `MemberCount`='".$totalg[0]."' WHERE `GuildName`='".$sqlly["GuildName"]."'") or die(mysql_error());
        }
    }
    echo "OK\n";
    echo "> Updating Guild Ranks based on Guild Score...";
    $gscore = mysql_query("SELECT `GuildId` FROM `Guild` ORDER BY `GuildScore` DESC");
    $grank = 0;
    while ($gscoreupdate = mysql_fetch_array($gscore)){
        $grank++;
        mysql_query("UPDATE `Guild` SET `GuildRank`='".$grank."' WHERE `GuildId`='".$gscoreupdate[0]."'") or die(mysql_error());   
    }
    echo "OK\n";

    echo ">> Guild Ranks Updated successfully.\n\n";

}

    /* Update Level & Award Page */
    echo "[Updating Level & Award page...]\n";
$level_tree = array (
    array("Battle Axe+", 7),
    array("Silver Battle Axe", 6),
    array("Silver Battle Axe+", 5),
    array("Gold Battle Axe", 4),
    array("Gold Battle Axe+", 3),
    array("Violet Wand", 2),
    array("Sapphire Wand", 1),
    array("Ruby Wand", 0),
    array("Diamond Wand", -1),
    array("Blue Dragon", -2),
    array("Red Dragon", -3),
    array("Silver Dragon", -4));

    $basegp = 6900;
    $sqlly_gp[0] = $basegp;
    foreach($level_tree as $lvupdate){
        echo "Now updating '".$lvupdate[0] . "'...";
        //First we need to check if there is even anyone at that rank.
        $sql_check = mysql_query("SELECT COUNT(*) FROM `Game` WHERE `TotalGrade`='".$lvupdate[1]."' AND `NoRankUpdate`=0");
        $sqlly_check = mysql_fetch_row($sql_check);
        if($sqlly_check[0] > 0){
            $sql_gp = mysql_query("SELECT `TotalScore` FROM `Game` WHERE `TotalGrade`='".$lvupdate[1]."' AND `NoRankUpdate`=0 ORDER BY `TotalRank` DESC LIMIT 1");
            $sqlly_gp = mysql_fetch_row($sql_gp);
            //Probably can update now.
            mysql_query("UPDATE `Ranks` SET `Gp`='".$sqlly_gp[0]."' WHERE `Id`='".$lvupdate[1]."'") or die(mysql_error());
            echo "...Updated to ".$sqlly_gp[0]." GP\n";
            $basegp = $sqlly_gp[0];
        } else {
            echo "...no users found, GP set to previous result (which is ".$sqlly_gp[0]." GP)\n";
            mysql_query("UPDATE `Ranks` SET `Gp`='".$sqlly_gp[0]."' WHERE `Id`='".$lvupdate[1]."'") or die(mysql_error());
        }
  
    }
    echo ">> Level & Award Page Updated Successfully.\n\n";


    echo "\n>>> SCRIPT COMPLETED SUCCESSFULLY";
?>
You must log in or register to reply here.
Share:
Share
 Trusted Releases
Contact usTerms and rulesPrivacy policyHelpHomeRSS
Community platform by XenForo® © 2010-2024 XenForo Ltd.
