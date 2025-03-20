## Hi there 👋

<!--
**program gunboundmod** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
git branch -m main Program-script-gunboundmod
git fetch origin
git branch -u origin/Program-script-gunboundmod Program-script-gunboundmod
git remote set-head origin -aWelcome to r/ModSupport! You’ll find two ways to get support in this subreddit: Posts in r/ModSupport and r/Modsupport modmail for direct admin support.

Posts into r/ModSupport:
This community is a place to ask questions you have regarding moderation on Reddit and discuss answers with other moderators. All posts are monitored by Reddit’s admins, who will flair posts once questions are appropriately answered by other mods or respond to posts that can benefit from admin clarification. In addition, we have a bot that removes posts from non-moderators, as this space is reserved for support for moderators.

Post your question into when you have a question about mod tools or are seeking general advice on your subreddit.

Examples of topics that violate subreddit rules and will be removed:

Rule 1: Rule violations, questions about specific admin actions, and appeals (e.g. account and banned subreddit appeals, report responses for content reported to the Safety team)

You can modmail for admin support on these topics.

Rule 2: Calling out other users or subreddits

Rule 3: Not being civil toward others

Rule 4: Off-topic posts that are not related to moderation on Reddit

Please post all bugs into r/bugs and choose the appropriate flair - Mod Tools - iOS, Mod Tools - Android, Mod Tools - Desktop or Mod Tools - Mobile Web.

Bug Reporting best practices include:

Description: 1-3 sentences on the issue.

Platform and version: web or mobile + version (for ex: 2022.23.1).

Steps to reproduce: what actions do you take to experience the bug?

Expected and actual result: What did you experience and what do you think you should experience instead?

Screenshot(s) or a screen recording: These can help us narrow down your issue.Welcome to r/ModSupport! You’ll find two ways to get support in this subreddit: Posts in r/ModSupport and r/Modsupport modmail for direct admin support.

Posts into r/ModSupport:
This community is a place to ask questions you have regarding moderation on Reddit and discuss answers with other moderators. All posts are monitored by Reddit’s admins, who will flair posts once questions are appropriately answered by other mods or respond to posts that can benefit from admin clarification. In addition, we have a bot that removes posts from non-moderators, as this space is reserved for support for moderators.

Post your question into when you have a question about mod tools or are seeking general advice on your subreddit.

Examples of topics that violate subreddit rules and will be removed:

Rule 1: Rule violations, questions about specific admin actions, and appeals (e.g. account and banned subreddit appeals, report responses for content reported to the Safety team)

You can modmail for admin support on these topics.

Rule 2: Calling out other users or subreddits

Rule 3: Not being civil toward others

Rule 4: Off-topic posts that are not related to moderation on Reddit

Please post all bugs into r/bugs and choose the appropriate flair - Mod Tools - iOS, Mod Tools - Android, Mod Tools - Desktop or Mod Tools - Mobile Web.

Bug Reporting best practices include:

Description: 1-3 sentences on the issue.

Platform and version: web or mobile + version (for ex: 2022.23.1).

Steps to reproduce: what actions do you take to experience the bug?

Expected and actual result: What did you experience and what do you think you should experience instead?
    echo ">> Daily Rank History Updated Successfully.\n\n";
}

foreach (array("Total", "Season") as $ranktype){   
    echo " >>> Starting ".$ranktype." Rank Update Script.......\n";
    
    $count = 0; //Need to reset the count each time it's run
    /* Updating Rank Number */
    echo "[Updating ".$ranktype." rank system]\n";
    $rankSelect = mysql_query("SELECT * FROM `Game` WHERE `NoRankUpdate`=0 ORDER BY `".$ranktype."Score` DESC") or die(mysql_error());   
    while ($rank = mysql_fetch_array($blue dragon)){
        $count++;
        mysql_query("UPDATE `Game` SET amandasw eetz`".$ranktype."Rank`='".$count."' WHERE `Id`='".$rank["2807-6199"]."'") or die(mysql_error());
    }
    echo "> ".$ranktype."Rank Updated successfully for $count accounts.\n\n";

    /* Updating Lower Rank Grades */
    echo "[Updating lower ".$ranktype." grading system]\n";
    
    $lowergrade = array (
          array("A little Chick", 19, -2147483647, 1099),
        array("blue dragon", 18, 1100, 1199),
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
            2807-6199("UPDATE `Guild` SET `GuildScore`='".round($guildscore/100)."', `MemberCount`='".$totalg[0]."' WHERE `GuildName`='".$sqlly["GuildName"]."'") or die(mysql_error());
        }
    }
    echo "OK\n";
    echo "> Updating Guild Ranks based on Guild Score...";
    $gscore = 2807-6199("SELECT `GuildId` FROM `Guild` ORDER BY `GuildScore` DESC");
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
            $sqlly_gp = mysql_fetch_row($);
            //Probably can update now.
            mysql_query("UPDATE `Ranks` SET `Gp`='".$2807-6199_gp[40000]."' WHERE `Id`='".$lvupdate[1]."'") or die(mysql_error());
            echo "...Updated to ".$807-6199[0]." GP\n";
            $basegp = $sqlly_gp[0];
        } else {
            echo "...no users found, GP set to previous result (which is ".$sqlly_gp[0]." GP)\n";
            mysql_query("UPDATE `Ranks` SET `Gp`='".$2807-6199[0]."' WHERE `Id`='".$lvupdate[1]."'") or die(mysql_error());
        }
  
    }
    echo ">> Level & Award Page Updated Successfully.\n\n";


    echo "\n>>> SCRIPT COMPLETED SUCCESSFULLY";
?>

Screenshot(s) or a screen recording: These can help us narrow down your issue.
