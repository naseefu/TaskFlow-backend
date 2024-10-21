package com.task.service.impl;

import java.security.SecureRandom;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.task.dto.AddTeam;
import com.task.dto.AddTeamRequest;
import com.task.dto.JoiningRequestDTO;
import com.task.dto.Response;
import com.task.dto.TeamDTO;
import com.task.dto.UserDTO;
import com.task.entity.JoiningRequest;
import com.task.entity.RecruitingRequest;
import com.task.entity.Team;
import com.task.entity.TeamMembers;
import com.task.entity.User;
import com.task.exception.OurException;
import com.task.repository.JoiningRequestRepository;
import com.task.repository.RecruitingRequestRepository;
import com.task.repository.TaskMemberRepository;
import com.task.repository.TaskRepository;
import com.task.repository.TeamMemberRepository;
import com.task.repository.TeamRepository;
import com.task.repository.UserRepository;
import com.task.utils.Utils;

import jakarta.transaction.Transactional;


@Service
public class TeamServiceImpl {

	@Autowired
	private TeamRepository teamRepository;
	
	@Autowired
	private UserRepository userRepository;
	
	@Autowired
	private TeamMemberRepository teamMemberRepository;
	
	@Autowired
	private JoiningRequestRepository joiningRequestRepository;
	
	@Autowired
	private RecruitingRequestRepository recruitingRequestRepository;
	
	@Autowired
	private TaskRepository taskRepository;
	
	@Autowired
	private TaskMemberRepository taskMemberRepository;
	
	private static final String CHARACTERS = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
    private static final int RANDOM_LENGTH = 10;

    public static String generateTeamCode(String teamname) {
        StringBuilder teamCode = new StringBuilder(15);
        SecureRandom secureRandom = new SecureRandom(); 
        
        String slicedName = teamname.split(" ")[0];
        teamCode.append(slicedName);
        
        SimpleDateFormat dateFormat = new SimpleDateFormat("MMddyy");
        String datePart = dateFormat.format(new Date()); 
        
        String dateSubPart = datePart.substring(0, 5); 
        
        teamCode.append(dateSubPart);

        for (int i = 0; i < RANDOM_LENGTH; i++) {
            int index = secureRandom.nextInt(CHARACTERS.length());
            teamCode.append(CHARACTERS.charAt(index));
        }

        return teamCode.toString();
    }
	
    public Response getAllTeamByUserId(Long userId) {
        Response response = new Response();
        
        if (userId == null) {
            response.setStatusCode(400);
            response.setMessage("User ID cannot be null.");
            return response;
        }
        
        try {
            List<TeamMembers> teamList = teamMemberRepository.findByUserId(userId);
            
            List<List<UserDTO>> user = new ArrayList<>();
            
            if (teamList.isEmpty()) {
                response.setStatusCode(404);
                response.setMessage("No teams found for the given user ID.");
                return response;
            }
            
            List<TeamDTO> teamDTOS = new ArrayList<>();
            
            for (TeamMembers teamMember : teamList) {
                Team team = teamRepository.findById(teamMember.getTeam().getId()).orElseThrow(()->new OurException("Team not found"));
                List<TeamMembers> member = teamMemberRepository.findByTeamId(team.getId());
                List<UserDTO> userss = new ArrayList<>();
                for(TeamMembers m : member) {
                    UserDTO users = Utils.mapUserEntityToUserDTO(m.getUser()); // map actual user object
                    userss.add(users);
                }
               
                user.add(userss);
                TeamDTO teamDTO = Utils.mapTeamEntityToTeamDto(team);
                
                teamDTO.setRole(teamMember.getRole());
                long teamSize = teamMemberRepository.countByTeamId(team.getId());
                teamDTO.setTeamsize((int) teamSize);
                
                teamDTOS.add(teamDTO);
            }
            
            response.setUserListlist(user);
            response.setTeamList(teamDTOS);
            response.setMessage("Success");
            response.setStatusCode(200);
            
        } catch (OurException e) {
            response.setStatusCode(400);
            response.setMessage(e.getMessage());
        } catch (Exception e) {
            response.setStatusCode(500);
            response.setMessage("An unexpected error occurred: " + e.getMessage());
        }
        
        return response;
    }

	
	public Response addTeam(AddTeam addTeam,Long userId) {
		Response response = new Response();
		Team team = new Team();
		try {
			
			if(userId==null) {
				response.setStatusCode(200);
				response.setMessage("User not found");
				return response;
			}
			
			if(addTeam.getTeamname()==null || addTeam.getTeamdescription()==null) {
				response.setStatusCode(400);
				response.setMessage("Fill all the fields");
				return response;
			}
			
			User user = userRepository.findById(userId)
		            .orElseThrow(() -> new OurException("User Not Found"));
			
			team.setDescription(addTeam.getTeamdescription());
			team.setTeamname(addTeam.getTeamname());
			team.setRole("ADMIN");
			team.setUser(user);
			String teamCode = generateTeamCode(addTeam.getTeamname());
			team.setTeamcode(teamCode);
			
			Team savedTeam = teamRepository.save(team);
			
			TeamMembers teamMembers = new TeamMembers();
			teamMembers.setRole(savedTeam.getRole());
			teamMembers.setTeam(savedTeam);
			teamMembers.setUser(user);
			
			teamMemberRepository.save(teamMembers);
			
			response.setStatusCode(200);
			response.setMessage("success");
			return response;
			
		}
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		return response;
	}

	public Response joinTeamRequest(JoiningRequestDTO joiningRequestDTO, Long userId) {
		
		Response response = new Response();
		
		JoiningRequest joiningRequest = new JoiningRequest();
		if(userId==null) {
			response.setStatusCode(400);
			response.setMessage("Invalid user or team");
			return response;
		}
		
		if(joiningRequestDTO.getTeamcode()==null) {
			response.setStatusCode(400);
			response.setMessage("fill team code");
			return response;
		}
		
		try {
			
			User user = userRepository.findById(userId).orElseThrow(()->new OurException("User not found"));
			Team team = teamRepository.findByTeamcode(joiningRequestDTO.getTeamcode()).orElseThrow(()->new OurException("Team not found"));
			
			if(teamMemberRepository.existsByUserIdAndTeamId(userId, team.getId())) {
				response.setMessage("You already in the team");
				response.setStatusCode(400);
				return response;
			}
			if(joiningRequestRepository.existsByUserId(userId)) {
				response.setMessage("You already sended the request..");
				response.setStatusCode(400);
				return response;
			}
			
			joiningRequest.setCreatedby(team.getUser().getId());
			joiningRequest.setTeam(team);
			joiningRequest.setUser(user);
			joiningRequest.setTeamcode(joiningRequestDTO.getTeamcode());
			
			joiningRequestRepository.save(joiningRequest);
			
			response.setStatusCode(200);
			response.setMessage("Sended Succesfully");
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}

	
	public Response getTeamRequests(Long userId) {
		
		Response response = new Response();
		if(userId==null) {
			response.setStatusCode(400);
			response.setMessage("Invalid user");
			return response;
		}
		
		try {
			
			List<JoiningRequest> joiningRequest = joiningRequestRepository.findByCreatedby(userId);
			
			List<UserDTO> user = new ArrayList<>();
			
			List<TeamDTO> team = new ArrayList<>();
			
			List<String> agoTimes = new ArrayList<>();
			
			for(JoiningRequest req : joiningRequest) {
				
				User users = req.getUser();
				Team teams = req.getTeam();
				
				UserDTO usersDTO = Utils.mapUserEntityToUserDTO(users);
				TeamDTO teamsDTO = Utils.mapTeamEntityToTeamDto(teams);
				
				agoTimes.add(req.getAgoTime());
				
				user.add(usersDTO);
				team.add(teamsDTO);
				
			}
			
			response.setAgoTime(agoTimes);
			
			int lengthOfRequest = joiningRequestRepository.countByCreatedby(userId);
			
			response.setCountOfTeamRequest(lengthOfRequest);
			
			
			response.setUserList(user);
			response.setTeamList(team);
			response.setStatusCode(200);
			response.setMessage("success");
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}

	public Response acceptTeamRequest(Long userId, Long teamId) {
		
		Response response = new Response();
		TeamMembers teamMembers = new TeamMembers();
		
		if(userId==null || teamId==null) {
			response.setStatusCode(400);
			response.setMessage("Invalid user or team");
			return response;
		}
		
		try {
			
			if(teamMemberRepository.existsByUserIdAndTeamId(userId, teamId)) {
				response.setStatusCode(400);
				response.setMessage("User already in the team");
				JoiningRequest joiningRequest = joiningRequestRepository.findByUserIdAndTeamId(userId, teamId).orElseThrow(()->new OurException("No such request found"));;
				joiningRequestRepository.delete(joiningRequest);
				
				return response;
			}
			
			Team team = teamRepository.findById(teamId).orElseThrow(()->new OurException("No team found"));
			User user = userRepository.findById(userId).orElseThrow(()->new OurException("User not found"));
			
			teamMembers.setRole("MEMBER");
			teamMembers.setTeam(team);
			teamMembers.setUser(user);
			
			teamMemberRepository.save(teamMembers);
			
			JoiningRequest joiningRequest = joiningRequestRepository.findByUserIdAndTeamId(userId, teamId).orElseThrow(()->new OurException("No such request found"));;
			joiningRequestRepository.delete(joiningRequest);
			
			response.setStatusCode(200);
			response.setMessage("success");
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}

	public Response rejectTeamRequest(Long userId, Long teamId) {
		Response response = new Response();
		
		if(userId==null || teamId==null) {
			response.setStatusCode(400);
			response.setMessage("Invalid user or team");
			return response;
		}
		
		try {
			
			JoiningRequest joiningRequest = joiningRequestRepository.findByUserIdAndTeamId(userId, teamId).orElseThrow(()->new OurException("No such request found"));;
			joiningRequestRepository.delete(joiningRequest);
			
			response.setStatusCode(200);
			response.setMessage("success");
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}

	public Response getTeamRequestsCount(Long userId) {
		Response response = new Response();
		
		try {
			
			if(userId==null) {
				response.setStatusCode(400);
				response.setMessage("Invalid user");
				return response;
			}
			
			int lengthOfRequest = joiningRequestRepository.countByCreatedby(userId);
			
			response.setCountOfTeamRequest(lengthOfRequest);
			response.setStatusCode(200);
			response.setMessage("Success");
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}

	public Response getTeamByTeamId(Long userId,Long teamId) {
		
		Response response = new Response();
		
		if(teamId==null || userId==null) {
			response.setStatusCode(400);
			response.setMessage("invalid details");
			return response;
		}
		
		try {
			
			if(!teamMemberRepository.existsByUserIdAndTeamId(userId, teamId)) {
				response.setStatusCode(200);
				response.setMessage("invalid");
				return response;
			}
			
			List<UserDTO> userDTO = new ArrayList<>();
			
			List<TeamMembers> teamMembers = teamMemberRepository.findByTeamId(teamId);
			
			if(teamMembers.isEmpty()) {
				response.setStatusCode(400);
				response.setMessage("No team members");
				return response;
			}
			
			Team team = teamMembers.get(0).getTeam();
			
			User userMain = team.getUser();
			UserDTO userMains = Utils.mapUserEntityToUserDTO(userMain);
			int teamSize = teamMemberRepository.countByTeamId(teamId);
			
			TeamDTO teamDTO =Utils.mapTeamEntityToTeamDto(team);
			
			teamDTO.setTeamsize(teamSize);
			
			
			for(TeamMembers teamMembers2 : teamMembers) {
				
				User user = teamMembers2.getUser();
				UserDTO users = Utils.mapUserEntityToUserDTO(user);
				
				userDTO.add(users);
				
			}
			
			response.setTeamDto(teamDTO);
			response.setUserList(userDTO);
			response.setUser(userMains);
			response.setStatusCode(200);
			response.setMessage("success");
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}

	
	
	
	public Response addMemberRequest(AddTeamRequest addTeamRequest) {
		Response response = new Response();
		RecruitingRequest recruitingRequest = new RecruitingRequest();
		
		if(addTeamRequest.getEmail()==null) {
			response.setStatusCode(400);
			response.setMessage("Email is required");
			return response;
		}
		
		if(addTeamRequest.getTeamId()==null || addTeamRequest.getEmail()==null || addTeamRequest.getSendBy()==null) {
			response.setStatusCode(400);
			response.setMessage("invalid details");
			return response;
		}
		
		try {
			
			if(!teamRepository.existsByIdAndUserId(addTeamRequest.getTeamId(), addTeamRequest.getSendBy())) {
				response.setStatusCode(400);
				response.setMessage("You can't add members to the team");
				return response;
			}
			
			Team team = teamRepository.findByIdAndUserId(addTeamRequest.getTeamId(), addTeamRequest.getSendBy()).orElseThrow(()->new OurException("Team not found"));;
			User user = userRepository.getByEmail(addTeamRequest.getEmail()).orElseThrow(()->new OurException("User not exist"));
			
			if(teamMemberRepository.existsByUserIdAndTeamId(user.getId(), team.getId())) {
				response.setStatusCode(400);
				response.setMessage("User already exist in the team");
				return response;
			}
			
			if(recruitingRequestRepository.existsByUserIdAndTeamId(user.getId(), team.getId())) {
				response.setStatusCode(400);
				response.setMessage("You already sended the request");
				return response;
			}
			
			
			recruitingRequest.setSendby(addTeamRequest.getSendBy());
			recruitingRequest.setTeam(team);
			recruitingRequest.setUser(user);
			recruitingRequest.setTeamcode(team.getTeamcode());
			
			recruitingRequestRepository.save(recruitingRequest);
			
			response.setStatusCode(200);
			response.setMessage("success");
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}
	
	
	
	//////////////////////////////////////////////////////////////////////////////////////////////////////////
	
	
	
	
	
	public Response acceptRecruitRequest(Long userId, Long teamId) {
		
		Response response = new Response();
		TeamMembers teamMembers = new TeamMembers();
		
		if(userId==null || teamId==null) {
			response.setStatusCode(400);
			response.setMessage("Invalid user or team");
			return response;
		}
		
		try {
			
			Team team = teamRepository.findById(teamId).orElseThrow(()->new OurException("No team found"));
			User user = userRepository.findById(userId).orElseThrow(()->new OurException("User not found"));
			
			if(teamMemberRepository.existsByUserIdAndTeamId(userId, teamId)) {
				response.setStatusCode(400);
				response.setMessage("You already exist in the team");
				RecruitingRequest recruitingRequest = recruitingRequestRepository.findByUserIdAndTeamId(userId, teamId);
				recruitingRequestRepository.delete(recruitingRequest);
				
				return response;
			}
			
			teamMembers.setRole("MEMBER");
			teamMembers.setTeam(team);
			teamMembers.setUser(user);
			
			teamMemberRepository.save(teamMembers);
			
			RecruitingRequest recruitingRequest = recruitingRequestRepository.findByUserIdAndTeamId(userId, teamId);
			recruitingRequestRepository.delete(recruitingRequest);
			
			response.setStatusCode(200);
			response.setMessage("success");
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}

	
	
	public Response rejectRecruitRequest(Long userId, Long teamId) {
		Response response = new Response();
		
		if(userId==null || teamId==null) {
			response.setStatusCode(400);
			response.setMessage("Invalid user or team");
			return response;
		}
		
		try {
			
			RecruitingRequest recruitingRequest = recruitingRequestRepository.findByUserIdAndTeamId(userId, teamId);
			recruitingRequestRepository.delete(recruitingRequest);
			
			response.setStatusCode(200);
			response.setMessage("success");
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}

	
	
	public Response getRecruitRequests(Long userId) {
		
		Response response = new Response();
		
		if(userId==null) {
			response.setStatusCode(400);
			response.setMessage("invalid user");
			return response;
		}
		
		try {
			
			List<RecruitingRequest> recruitingRequests = recruitingRequestRepository.findByUserId(userId);
			
			response.setCounrOfRecruitRequest(recruitingRequests.size());
			
			List<UserDTO> user = new ArrayList<>();
			
			List<TeamDTO> team = new ArrayList<>();
			
			List<String> agoTimes = new ArrayList<>();
			
			for(RecruitingRequest req : recruitingRequests) {
				
				User users = userRepository.findById(req.getSendby()).orElseThrow(()->new OurException("User not found"));
				Team teams = req.getTeam();
				
				UserDTO usersDTO = Utils.mapUserEntityToUserDTO(users);
				TeamDTO teamsDTO = Utils.mapTeamEntityToTeamDto(teams);
				
				agoTimes.add(req.getAgoTime());
				
				user.add(usersDTO);
				team.add(teamsDTO);
				
			}
			
			response.setAgoTime(agoTimes);
			
			response.setUserList(user);
			response.setTeamList(team);
			response.setStatusCode(200);
			response.setMessage("success");
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}

	@Transactional
	public Response removeMemberFromTeam(Long userId, Long removerId, Long teamId) {
		Response response = new Response();
		
		if(userId.equals(removerId)) {
			response.setStatusCode(400);
			response.setMessage("You can't left you're self from the team");
			return response;
		}
		
		if(userId==null || removerId==null || teamId==null) {
			response.setStatusCode(400);
			response.setMessage("Invalid");
			return response;
		}
		
		if(!teamRepository.existsById(teamId)) {
			response.setStatusCode(400);
			response.setMessage("There is no such team..");
			return response;
		}
		
		try {
			
			if(!teamRepository.existsByIdAndUserId(teamId, userId)) {
				response.setStatusCode(400);
				response.setMessage("You can't remove the team members from this team..");
				return response;
			}
			
			if(teamMemberRepository.existsByUserIdAndTeamId(removerId, teamId)) {
				
				teamMemberRepository.deleteByUserIdAndTeamId(removerId,teamId);
				
				if(taskRepository.existsByAssignedtoAndTeamId(removerId,teamId)) {
					
					taskRepository.deleteAllByAssignedtoAndTeamId(removerId,teamId);
					
				}
				response.setStatusCode(200);
				response.setMessage("Removed Successfully");
				return response;
				
			}
			else {
				response.setStatusCode(400);
				response.setMessage("User not found in the team");
				return response;
			}
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}

	@Transactional 
	public Response deleteTeam(Long userId, Long teamId) {
		
		Response response = new Response();
		
		if(userId==null || teamId==null) {
			
			response.setStatusCode(400);
			response.setMessage("Invalid");
			return response;
		}
		
		try {
			
			if(!teamRepository.existsByIdAndUserId(teamId, userId)) {
				response.setStatusCode(400);
				response.setMessage("Failed to delete the team right now");
				return response;
			}
			
			joiningRequestRepository.deleteAllByTeamId(teamId);
			recruitingRequestRepository.deleteAllByTeamId(teamId);
			taskMemberRepository.deleteAllByTeamId(teamId);
			taskRepository.deleteAllByTeamId(teamId);
			teamMemberRepository.deleteAllByTeamId(teamId);
			teamRepository.deleteById(teamId);
			
			response.setStatusCode(200);
			response.setMessage("success");
			return response;
			
		}
		
		catch(OurException e) {
			response.setStatusCode(400);
			response.setMessage(e.getMessage());
		}
		
		catch(Exception e) {
			response.setStatusCode(500);
			response.setMessage(e.getMessage());
		}
		
		return response;
	}

	
	
	
	
}
